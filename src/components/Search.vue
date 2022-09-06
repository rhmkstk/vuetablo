<script>
export default {
  name: "serch",
  props: {
    filterField: {
      type: [String, Number],
      required: true,
    },
    placeholder: {
      type: String,
      required: false,
      default: "Search",
    },
  },
  data() {
    return {
      filterActiveClass: false,
      filter: this.filterField,
    };
  },
  watch: {
    filter: function() {
      this.$emit("update:filterField", this.filter);
    },
  },
};
</script>
<template>
  <section class="search">
    <div class="searchContainer">
      <i>
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="22"
          height="22"
          viewBox="0 0 24 24"
          stroke-width="1.5"
          stroke="currentColor"
          fill="none"
          stroke-linecap="round"
          stroke-linejoin="round"
        >
          <path stroke="none" d="M0 0h24v24H0z" fill="none" />
          <circle cx="10" cy="10" r="7" />
          <line x1="21" y1="21" x2="15" y2="15" />
        </svg>
      </i>
      <input type="text" v-model="filter" :placeholder="placeholder" />
    </div>
  </section>
</template>
<style scoped>
.searchContainer {
  position: relative;
  display: flex;
  align-items: center;
  min-width: 300px;
  margin: 25px 25px 10px 25px;
  background: none;
}
.searchContainer input {
  width: 100%;
  background: var(--vue-data-table-bg);
  border: none;
  border-bottom: 1px solid var(--vue-data-table-border-color);
  padding: 10px 35px;
  position: relative;
  font-size: 1rem;
  color: var(--vue-data-table-text-color);
  outline: none;
  transition: border-color 300ms;
}
.searchContainer input:focus {
  border-color: var(--vue-data-table-text-color);
}
.searchContainer i {
  position: absolute;
  left: 0;
  display: inline-flex;
  margin-left: 5px;
  z-index: 4;
  color: var(--vue-data-table-text-color);
}
</style>
