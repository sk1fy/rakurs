<template>
  <button
      :disabled="isDisabled"
      @mouseover="hover = true"
      @mouseleave="hover = false"
      @mousedown="pressed = true"
      @mouseup="pressed = false"
  >
      <slot></slot>
  </button>
</template>

<script>
  export default {
    data() {
      return {
        hover: false,
        pressed: false,
        isDisabled: this.$props.disabled || false
      };
    },
    props: {
      disabled: {
        type: Boolean
      }
    }
  };
  </script>

<style scoped lang="scss">
button {
  background-color: #17505B; /* Измените на цвет по умолчанию */
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  height: 40px;
  width: 100%;
}

button:hover {
  background-color: #5D858D;
}

button:disabled {
  background-color: #8BA8AD;
  color: #fff;
  cursor: not-allowed;
}

button:active {
  background-color: #124049;
}
</style>
