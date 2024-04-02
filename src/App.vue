<template>
  <div id="app">
    <RaForm></RaForm>
    <div>
      <!-- <input type="text" class="input-field" placeholder="Text">
      <input type="text" class="input-field filled" placeholder="Text" value="Filled Value">
      <input type="text" class="input-field error" placeholder="Text">
      <input type="text" class="input-field" placeholder="Text" disabled> -->
    </div>
  </div>
</template>

<script>
import RaForm from './components/RaForm.vue'


export default {
  name: 'App',
  components: {
    RaForm
  }
}
</script>

<style lang="scss">
@import './assets/styles/Input.scss';
body {
  font-family: 'Roboto', sans-serif;
}
</style>

