<script>
import AppHeader from "@/components/AppHeader.vue";
export default {
  components: {
    AppHeader,
  },
};
</script>
<template>
  <div id="app">
    <AppHeader />
    <div class="container-page mt-3">
      <router-view />
    </div>
  </div>
</template>
<style>
.container-page .page{
    width: 300px;
}
</style>
