<template>
  <button
    :class="['btn', colorClass, sizeClass]"
    :type="type"
    :disabled="disabled"
    @click="handleClick"
  >
    <span v-if="icon" class="icon">
      <i :class="icon"></i>
    </span>
    {{ label }}
  </button>
</template>

<script>
export default {
  props: {
    label: {
      type: String,
      default: 'Click',
    },
    type: {
      type: String,
      default: 'button',
    },
    disabled: {
      type: Boolean,
      default: false,
    },
    color: {
      type: String,
      default: 'blue',
    },
    size: {
      type: String,
      default: 'medium',
    },
    icon: {
      type: String,
      default: '',
    },
  },
  computed: {
    colorClass() {
      return `btn-${this.color}`;
    },
    sizeClass() {
      return `btn-${this.size}`;
    },
  },
  methods: {
    handleClick() {
      this.$emit('click');
    },
  },
};
</script>

<style scoped>
.btn-red {
  background-color: red;
  color: white;
}
.btn-blue {
  background-color: blue;
  color: white;
}
.btn-small {
  font-size: 12px;
  padding: 6px 12px;
}
.btn-medium {
  font-size: 14px;
  padding: 8px 16px;
}
.btn-large {
  font-size: 16px;
  padding: 10px 20px;
}
.icon {
  margin-right: 8px;
}
</style>
