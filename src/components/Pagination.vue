<script>
export default {
  name: "tableFooter",
  props: {
    perPageDefault: {
      type: Number,
      default: 5,
    },
    options: {
      type: Array,
      default: () => [{ value: 5 }, { value: 10 }, { value: 15 }],
    },
    startPointToShowItems: {
      type: Number,
      required: true,
      default: 0,
    },
    tableDataLength: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      selectedPage: this.perPageDefault,
      startPoint: this.startPointToShowItems,
    };
  },
  watch: {
    selectedPage: function() {
      this.$emit("update:perPageDefault", this.selectedPage);
    },
  },
  methods: {
    nextItems() {
      this.startPoint += this.selectedPage;
      this.$emit("update:startPointToShowItems", this.startPoint);
    },
    prevItems() {
      this.startPoint -= this.selectedPage;
      this.$emit("update:startPointToShowItems", this.startPoint);
    },
  },
  computed: {
    isFooterButtonDisabled() {
      let obj = { prev: true, next: false };
      const start = this.startPointToShowItems;
      const selected = this.perPageDefault;
      const length = this.tableDataLength;
      if (start > 0) {
        obj.prev = false;
      }
      if (start + selected >= length) {
        obj.next = true;
      }
      return obj;
    },
  },
};
</script>

<template>
  <div class="footer-container">
    <span class="title">Rows per page:</span>
    <select v-model="selectedPage">
      <option
        v-for="option in options"
        :key="option.value"
        :value="option.value"
      >
        {{ option.value }}
      </option>
    </select>
    <div class="footer-icons">
      <button :disabled="isFooterButtonDisabled.prev" @click="prevItems">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="24"
          height="24"
          viewBox="0 0 24 24"
          stroke-width="1.5"
          stroke="currentColor"
          fill="none"
          stroke-linecap="round"
          stroke-linejoin="round"
        >
          <path stroke="none" d="M0 0h24v24H0z" fill="none" />
          <polyline points="15 6 9 12 15 18" />
        </svg>
      </button>
      <button :disabled="isFooterButtonDisabled.next" @click="nextItems">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="24"
          height="24"
          viewBox="0 0 24 24"
          stroke-width="1.5"
          stroke="currentColor"
          fill="none"
          stroke-linecap="round"
          stroke-linejoin="round"
        >
          <path stroke="none" d="M0 0h24v24H0z" fill="none" />
          <polyline points="9 6 15 12 9 18" />
        </svg>
      </button>
    </div>
  </div>
</template>

<style scoped>
.footer-container {
  padding: 15px;
  width: 320px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-left: auto;
}
.footer-container span {
  color: var(--vue-data-table-text-color);
}
.footer-container select {
  background: var(--vue-data-table-stripe-color);
  padding: 4px;
  border-color: var(--vue-data-table-border-color);
  border-radius: 4px;
  font-size: 1rem;
  color: var(--vue-data-table-text-color);
}
.footer-container .footer-icons button {
  border: none;
  background: none;
  padding: 0;
  margin: 0;
  cursor: pointer;
  border-radius: 99px;
  width: 40px;
  height: 40px;
  transition: background-color 300ms;
  color: var(--vue-data-table-text-color);
}
.footer-container .footer-icons button:hover {
  background-color: var(--vue-data-table-stripe-color);
}
.footer-container .footer-icons button:disabled {
  color: var(--vue-data-table-border-color);
  cursor: initial;
  background: transparent;
}
.footer-container .footer-icons button:first-child {
  margin-right: 10px;
}
</style>
