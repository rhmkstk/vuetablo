<script>
export default {
  name: "editCell",
  props: {
    value: {
      type: [String, Number],
    },
  },
  data() {
    return {
      edit: false,
      newVal: this.value,
      valCache: this.value,
    };
  },
  methods: {
    startEditing() {
      this.edit = !this.edit;
      setTimeout(() => {
        this.$refs["editInput"].focus();
      }, 100);
    },
    onUpdateValue() {
      this.edit = !this.edit;
      this.$emit("onUpdateValue", this.newVal);
    },
    onCancelEdit() {
      this.edit = !this.edit;
      this.newVal = this.valCache;
    },
  },
};
</script>
<template>
  <span class="table-cell">
    <span v-if="!edit" @click="startEditing">{{ newVal }}</span>
    <input
      v-else
      class="edit"
      v-model="newVal"
      ref="editInput"
      type="text"
      @keyup.enter="onUpdateValue"
      @keyup.esc="onCancelEdit"
      @blur="edit = false"
    />
  </span>
</template>
<style scoped>
.edit {
  padding: 4px 4px 4px 8px;
  max-width: 120px;
  border-radius: 5px;
  border: none;
  font-size: 1rem;
  font-family: inherit;
  background: var(--vue-data-table-bg);
  color: var(--vue-data-table-text-color);
  outline-color: var(--vue-data-table-text-color);
}
</style>
