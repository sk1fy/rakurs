<template>
  <div style="width: 100%;">
    <input :class="['input-field', { 'filled': filled, 'error': error }]" type="text" :placeholder="placeholder" v-model="inputValue" :disabled="disabled">
  </div>
</template>

<script>
  export default {
    props: {
      filled: {
        type: Boolean,
        default: false
      },
      error: {
        type: Boolean,
        default: false
      },
      disabled: {
        type: Boolean,
        default: false
      },
      placeholder: {
        type: String,
        default: ''
      },
      value: {
        type: String,
        default: ''
      }
    },
    data() {
      return {
        inputValue: this.$props.value
      };
    }
  };
</script>

<style lang="scss" scoped>
.input-field {
  width: -webkit-fill-available;
}
.error {
  border-color: red;
}
</style>
