<script>
export default {
  name: "sortableTitle",
  props: {
    title: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      visibility: {
        up: false,
        down: false,
      },
    };
  },
  methods: {
    //this function executes from main component..
    isIconVisible(title, direction) {
      if (title == this.title) {
        if (direction == "smtobig") {
          this.visibility.down = false;
          this.visibility.up = true;
        } else if (direction == "reverse") {
          this.visibility.up = false;
          this.visibility.down = true;
        }
      } else {
        this.visibility.up = false;
        this.visibility.down = false;
      }
    },
  },
};
</script>

<template>
  <span class="sortableTitle">
    <span class="wrapper">
      <span>{{ title }}</span>
      <i
        class="arrowUp"
        v-if="!visibility.down"
        :class="visibility.up ? 'iconVisible' : ''"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 24 24"
          class="arrow-up"
          stroke-width="1"
          stroke="currentColor"
          fill="none"
          stroke-linecap="round"
          stroke-linejoin="round"
        >
          <path stroke="none" d="M0 0h24v24H0z" fill="none" />
          <line x1="12" y1="5" x2="12" y2="19" />
          <line x1="16" y1="9" x2="12" y2="5" />
          <line x1="8" y1="9" x2="12" y2="5" />
        </svg>
      </i>
      <i v-else :class="visibility.down ? 'iconVisible' : ''">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="22"
          height="22"
          viewBox="0 0 24 24"
          class="arrow-up"
          stroke-width="1"
          stroke="currentColor"
          fill="none"
          stroke-linecap="round"
          stroke-linejoin="round"
        >
          <path stroke="none" d="M0 0h24v24H0z" fill="none" />
          <line x1="12" y1="5" x2="12" y2="19" />
          <line x1="16" y1="15" x2="12" y2="19" />
          <line x1="8" y1="15" x2="12" y2="19" />
        </svg>
      </i>
    </span>
  </span>
</template>

<style scoped>
.wrapper {
  display: inline-flex;
  align-items: center;
  margin-right: -22px;
}
.sortableTitle i {
  display: inline-flex;
  color: var(--vue-data-table-text-color);
  opacity: 0;
  visibility: hidden;
  transition: opacity 200ms;
  width: 22px;
  height: 22px;
}
.sortableTitle:hover i.arrowUp {
  opacity: 1;
  visibility: visible;
}
.iconVisible {
  opacity: 1 !important;
  visibility: visible !important;
}
</style>
