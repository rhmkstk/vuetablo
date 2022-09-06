<script>
import EditableCell from "./EditCell.vue";
import Search from "./Search";
import SortableTitle from "./SortableTitle";
import Pagination from "./Pagination";
export default {
  name: "CustomTable",
  components: {
    EditableCell,
    Search,
    SortableTitle,
    Pagination,
  },
  props: {
    titles: {
      type: Array,
      required: true,
    },
    tableData: {
      type: Array,
      required: true,
    },
    // tableComponents: {
    //   type: [Array, Object],
    //   required: false,
    // },
    search: {
      type: Boolean,
      default: true,
      required: false,
    },
    pagination: {
      type: Boolean,
      default: true,
      required: false,
    },
    tableStyles: {
      type: Array,
      required: false,
      default: () => []
    },
  },
  data() {
    return {
      filter: "",
      tableDataClone: this.tableData,
      // selectedPageCount: this.setColumnSize(),
      startPointToShowItems: 0,
    };
  },
  watch: {
    selectedPageCount: function() {
      this.startPointToShowItems = 0;
    },
  },
  methods: {
    renderCell(title, item) {
      let value = item[title.field];
      if (typeof title['formatter'] !== 'undefined') {
        const formatter = title.formatter;
        if (typeof formatter == "string") {
          const globalFilter = this.$options.filters[formatter];
          value = globalFilter(value);
        } else if (typeof formatter == "function") {
          value = formatter(value);
        }
      }
      if (title.suffix) {
        return value + title.suffix;
      } else if (title.prefix) {
        return title.prefix + value;
      } else {
        return value;
      }
    },
    setSortIconsVisibility(title, direction) {
      //reaches the sortableTitle component with ref and runs isIconVisible function which sortableTitle component has, calls from sortTable
      const titles = this.$refs.sortableTitle;
      titles.forEach((t) => {
        t.isIconVisible(title, direction);
      });
    },
    sortTable(title) {
      if (this.isSorted(this.tableDataClone, title.field)) {
        this.setSortIconsVisibility(title.title, "reverse");
        this.tableDataClone.reverse();
        return;
      }
      this.customSorter(this.tableDataClone, title.field);
      this.setSortIconsVisibility(title.title, "smtobig");
    },
    customSorter(arr, field) {
      arr.sort((a, b) =>
        a[field] > b[field] ? 1 : b[field] > a[field] ? -1 : 0
      );
    },
    isSorted(arr, field) {
      return arr.every((v, i, a) => !i || a[i - 1][field] <= v[field]);
    },
    checkAllFields(item) {
      if (!this.filterableFields.length) return true;
      let match = false;
      this.filterableFields.forEach((field) => {
        if (typeof item[field] == "string") {
          if (item[field].toLowerCase().indexOf(this.filter.toLowerCase()) > -1)
            match = true;
        }
        if (typeof item[field] == "number") {
          if (item[field].toString().indexOf(this.filter.toString()) > -1)
            match = true;
        }
      });
      return match;
    },
    onEmit(t, i) {
      this.$emit(`${t.click}`, t, i);
    },
    handleUpdateValue(newVal, item, title) {
      this.$emit("onUpdateValue", newVal, title, item);
    },
    hasFieldProperty(title, property) {
      // return title.hasOwnProperty(property);
      return typeof title[property] !== "undefined";
    },
    clickableElementsClass(title) {
      if (this.hasFieldProperty(title, "click") || title.editable) {
        return "__vue-data-table-pointer";
      }
      return "";
    },
    setUserComponentProp(title, item) {
      if (this.hasFieldProperty(title, "waitProp")) {
        return { [title.waitProp]: item[title.field] };
      }
      return undefined;
    },
    customStyle(title, item) {
      if (this.hasFieldProperty(title, "customStyle")) {
        const styleType = typeof title.customStyle;
        return styleType == "function"
          ? title.customStyle(item)
          : title.customStyle;
      }
      return "";
    },
    setSearchProp() {
      if (
        typeof this.tableComponents == "undefined" ||
        Array.isArray(this.tableComponents)
      ) {
        return undefined;
      } else {
        return this.tableComponents.search;
      }
    },
    // setColumnSize() {
    //   if (
    //     typeof this.tableComponents == "undefined" ||
    //     Array.isArray(this.tableComponents)
    //   ) {
    //     return 5;
    //   } else {
    //     return this.tableComponents.pagination.perPageDefault;
    //   }
    // },
    // setPaginationProp() {
    //   if (
    //     typeof this.tableComponents == "undefined" ||
    //     Array.isArray(this.tableComponents)
    //   ) {
    //     return undefined;
    //   } else {
    //     this.selectedPageCount = this.tableComponents.pagination.perPageDefault;
    //     return this.tableComponents.pagination;
    //   }
    // },
  },
  computed: {
    filteredTableData() {
      return this.tableDataClone
        .filter((item) => {
          if (this.checkAllFields(item)) return item;
        })
        .slice(
          this.startPointToShowItems,
          this.selectedPageCount + this.startPointToShowItems
        );
    },
    filterableFields() {
      return this.titles
        .map((data) => (data.filterable ? data.field : null))
        .filter((i) => i != null);
    },
    formatTableStyles() {
      return this.tableStyles.map((style) => `__vue-data-table-${style}`);
    },
    // isComponentUsing() {
    //   let c = { search: false, pagination: false };
    //   if (typeof this.tableComponents == "undefined") {
    //     return c;
    //   } else if (Array.isArray(this.tableComponents)) {
    //     if (this.tableComponents.includes("search")) {
    //       c.search = true;
    //     }
    //     if (this.tableComponents.includes("pagination")) {
    //       c.pagination = true;
    //     }
    //     return c;
    //   } else {
    //     if (this.tableComponents.hasOwnProperty("search")) {
    //       c.search = true;
    //     }
    //     if (this.tableComponents.hasOwnProperty("pagination")) {
    //       c.pagination = true;
    //     }
    //     return c;
    //   }
    // },
  },
};
</script>

<template>
  <div class="__vue-data-table-container" :class="formatTableStyles">
    <div class="__vue-data-table-search">
      <slot name="before-search" />
      <Search v-bind:filterField.sync="filter" v-bind="setSearchProp()" />
      <slot name="after-search" />
    </div>
    <table>
      <thead>
        <tr>
          <th
            v-for="(title, index) in titles"
            :key="index"
            v-on="title.sortable ? { click: () => sortTable(title) } : {}"
            :style="title.sortable ? 'cursor:pointer;' : ''"
          >
            <SortableTitle
              v-if="hasFieldProperty(title, 'sortable')"
              :title="title.title"
              ref="sortableTitle"
            />
            <span v-else>{{ title.title }}</span>
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in filteredTableData" :key="index">
          <template v-for="(title, index) in titles">
            <td
              :key="index"
              v-on="
                hasFieldProperty(title, 'click')
                  ? { click: () => onEmit(title, item) }
                  : {}
              "
              :class="clickableElementsClass(title)"
              :style="customStyle(title, item)"
            >
              <i
                v-if="hasFieldProperty(title, 'renderIcon')"
                :class="title.renderIcon"
              >
              </i>
              <component
                v-else-if="hasFieldProperty(title, 'renderComponent')"
                :is="title.renderComponent"
                v-bind="setUserComponentProp(title, item)"
              >
                <span>{{ item[title.field] }}</span>
              </component>

              <EditableCell
                v-else-if="hasFieldProperty(title, 'editable')"
                :value="renderCell(title, item)"
                @onUpdateValue="
                  (newVal) => {
                    handleUpdateValue(newVal, item, title);
                  }
                "
              />
              <span v-else class="__vue-data-table-table-cell">
                {{ renderCell(title, item) }}
              </span>
            </td>
          </template>
        </tr>
      </tbody>
    </table>
    <div class="__vue-data-table-pagination">
      <slot name="before-footer"></slot>
      <!-- <Pagination
        v-bind:perPageDefault.sync="selectedPageCount"
        v-bind:startPointToShowItems.sync="startPointToShowItems"
        :tableDataLength="tableDataClone.length"
        v-bind="setPaginationProp()"
      /> -->
      <slot name="after-footer"></slot>
    </div>
  </div>
</template>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
* {
  box-sizing: border-box !important;
}
table,
caption,
tbody,
tfoot,
thead,
tr,
th,
td {
  font-size: 100%;
}
table {
  border-collapse: collapse;
  width: 100%;
  overflow: scroll;
  text-indent: 0;
  font-family: inherit;
  text-align: center;
}
/* RESET */
:root {
  --vue-data-table-bg-color: #fff;
  --vue-data-table-text-color: #212529;
  --vue-data-table-border-color: rgba(0, 0, 0, 0.1);
  --vue-data-table-stripe-color: rgba(0, 0, 0, 0.05);
  --vue-data-table-shadow-color: rgba(83, 83, 83, 0.4);
}
.__vue-data-table-container table {
  border-radius: 14px;
}
.__vue-data-table-container table thead tr {
  font-weight: bold;
}

.__vue-data-table-container table thead tr th {
  padding: 13px 0px;
}
.__vue-data-table-container table tbody tr td {
  padding: 13px 0px;
}
.__vue-data-table-container.__vue-data-table-border table thead tr {
  border-bottom: 1px solid var(--vue-data-table-border-color);
}
.__vue-data-table-container.__vue-data-table-border table tbody tr {
  border-bottom: 1px solid var(--vue-data-table-border-color);
}

.__vue-data-table-container.__vue-data-table-border
  .__vue-data-table-pagination {
  border-bottom: 1px solid var(--vue-data-table-border-color);
}
.__vue-data-table-container.__vue-data-table-border-outside {
  border: 1px solid var(--vue-data-table-border-color);
}
.__vue-data-table-container.__vue-data-table-border-outside
  .__vue-data-table-pagination {
  border: none !important;
}

.__vue-data-table-container.__vue-data-table-stripe
  table
  tbody
  tr:nth-child(odd) {
  background-color: var(--vue-data-table-stripe-color);
}

.__vue-data-table-container.__vue-data-table-hover table tbody tr:hover {
  background-color: var(--vue-data-table-stripe-color);
}

.__vue-data-table-container.__vue-data-table-large table thead tr th,
.__vue-data-table-container.__vue-data-table-large table tbody tr td {
  padding: 18px 0px;
}
.__vue-data-table-container.__vue-data-table-small table thead tr th,
.__vue-data-table-container.__vue-data-table-small table tbody tr td {
  padding: 8px 4px;
}

.__vue-data-table-container.__vue-data-table-radius {
  border-radius: 14px;
}
.__vue-data-table-container.__vue-data-table-radius table tbody tr:last-child {
  border: none !important;
}

.__vue-data-table-container.__vue-data-table-shadow {
  box-shadow: 2px 2px 6px -1px var(--vue-data-table-shadow-color);
}
.__vue-data-table-container.__vue-data-table-align-left table {
  text-align: left;
}
.__vue-data-table-container.__vue-data-table-align-right table {
  text-align: right;
}

.__vue-data-table-search,
.__vue-data-table-pagination {
  display: flex;
  align-items: center;
}
.__vue-data-table-pointer {
  cursor: pointer !important;
}
</style>
