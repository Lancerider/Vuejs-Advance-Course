<template lang="pug">
#app
  PmHeader
  router-view
  PmFooter
</template>


<script>
import PmFooter from "@/components/layout/Footer.vue";
import PmHeader from "@/components/layout/Header.vue";

export default {
  name: "app",

  components: { PmFooter, PmHeader }
};
</script>
<style lang="scss">
@import "./scss/main.scss";
@import "./assets/css/fontawesome.min.css";
@import "./assets/css/solid.min.css";

.results {
  margin-top: 50px;
}
small {
  display: flex;
  text-align: center;
  width: 100%;
}
.is-active {
  border: 1px #aaa solid;
  background-color: #f5f5ff;
}
</style>
