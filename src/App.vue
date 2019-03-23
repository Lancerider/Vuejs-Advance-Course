<template lang="pug">
#app
  PmHeader

  PmLoader(v-show="isLoading")
  section.section(v-show="!isLoading")
    nav.navbar.has-shadow
      div.container
        input.input.is-large(
          type="text",
          placeholder="Buscar canciones",
          v-model="searchQuery"
        )
        a.button.is-info.is-large(v-on:click="search") Buscar
        a.button.is-danger.is-large &times;

    .container
      p
        small {{searchMessage}}

    div.container.results
      div.columns.is-multiline
        div.column.is-one-quarter(
          v-for="track in tracks")
          pm-track(v-bind:track="track")

  PmFooter
</template>


<script>
import trackService from "@/services/track";
import PmFooter from "@/components/layout/Footer.vue";
import PmHeader from "@/components/layout/Header.vue";

import PmTrack from "@/components/layout/Track.vue";
import PmLoader from "@/components/shared/Loader.vue";

export default {
  name: "app",

  components: { PmFooter, PmHeader, PmTrack, PmLoader },

  data() {
    return {
      searchQuery: "",
      tracks: [],

      isLoading: false
    };
  },

  methods: {
    search() {
      if (!this.searchQuery) {
        return;
      }
      this.isLoading = true;
      trackService.search(this.searchQuery).then(res => {
        this.tracks = res.tracks.items;
        this.isLoading = false;
      });
    }
  },

  computed: {
    searchMessage() {
      return `Encontrados: ${this.tracks.length}`;
    }
  }
};
</script>
<style lang="scss">
@import "./scss/main.scss";

.results {
  margin-top: 50px;
}
small {
  display: flex;
  text-align: center;
  width: 100%;
}
</style>
