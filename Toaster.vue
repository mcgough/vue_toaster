<template>
  <transition-group
    tag="ul"
    class="items"
    enter-active-class="animated faster fadeInRight"
    leave-active-class="animated faster fadeOutRight"
  >
    <li
      v-for="item in items"
      v-timer="{ timer, cb: () => removeItem(item.id) }"
      :key="item.id"
      :id="item.id"
      class="item"
    >
      <div :class="`toast-content ${item.type}`">
        <slot :item="item"></slot>
        <button type="button" @click="() => removeItem(item.id)">X</button>
      </div>
    </li>
  </transition-group>
</template>

<script>
export default {
  name: "toaster",
  directives: {
    timer: {
      bind: (el, { value: { timer, cb } }) => setTimeout(cb, timer)
    }
  },
  props: { item: Object, timer: Number },
  data: () => ({
    items: []
  }),
  watch: {
    item(latest) {
      if (latest && latest.id) this.items.unshift(latest);
    }
  },
  methods: {
    removeItem(id) {
      this.items = this.items.filter(item => item.id !== id);
    }
  }
};
</script>

<style scoped lang="scss">
ul.items {
  z-index: 1;
  position: fixed;
  top: 0;
  right: 0;
  padding: 0 1rem;
  overflow: hidden;
  overflow-y: auto;
}
li.item {
  list-style-type: none;
}
li .toast-content {
  display: flex;
  align-items: center;
  justify-content: space-between;
  min-width: 15rem;
  color: #fff;
  text-align: center;
  position: relative;
  border-radius: 1rem;
  padding: 1rem;
  margin-bottom: 15px;
  box-shadow: 2px 4px 5px 0px rgba(0, 0, 0, 0.25);
  &.success {
    background-color: rgba(69, 220, 53, 0.9);
  }

  &.danger {
    background-color: rgba(220, 53, 69, 0.9);
  }
}
button {
  color: #fff;
  border: none;
  background: none;
  font-size: 1.6rem;
  margin-left: 2rem;
  cursor: pointer;
}
</style>
