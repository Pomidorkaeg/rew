<template>
  <div id="app">
    <OrdersView />
  </div>
</template>

<script>
import OrdersView from './views/OrdersView.vue';

export default {
  name: 'App',
  components: {
    OrdersView
  }
};
</script>

<style lang="scss">
#app {
  min-height: 100vh; // Чтобы страница занимала всю высоту
}
</style>