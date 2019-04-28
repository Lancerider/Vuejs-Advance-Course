<template lang="pug">
main
  transition(name="move")
    PmNotification(v-show="showNotification")
      p(slot="body") No se encontraron resultados.

  transition(name="move")
    PmLoader(v-show="isLoading")
  section.section(v-show="!isLoading")
    nav.navbar
      div.container
        div.column.is-9
          input.input.is-large(
            type="text",
            placeholder="Buscar canciones",
            v-model="searchQuery"
            @keyup.enter="search"
          )
        div.column.is-1
          a.button.is-info.b_search.is-large(v-on:click="search") Buscar
        div.column.is-1
          a.button.is-danger.is-large(@click="cleanSearch") &times;

    .container
      p
        small {{searchMessage}}

    div.container.results
      div.columns.is-multiline
        div.column.is-one-quarter(
          v-for="track in tracks")
          pm-track(
            v-blur="track.preview_url",
            :class="{'is-active':track.id===selectedTrack}",
            v-bind:track="track",
            @select="setSelectedTrack"
            )
</template>


<script>
import trackService from "@/services/track";

import PmTrack from "@/components/Track.vue";

import PmLoader from "@/components/shared/Loader.vue";
import PmNotification from "@/components/shared/Notification.vue";

export default {
  name: "app",

  components: { PmTrack, PmLoader, PmNotification },

  data() {
    return {
      searchQuery: "",
      tracks: [],

      isLoading: false,
      showNotification: false,

      selectedTrack: ""
    };
  },

  methods: {
    search() {
      if (!this.searchQuery) {
        return;
      }
      this.isLoading = true;
      trackService.search(this.searchQuery).then(res => {
        this.showNotification = res.tracks.total === 0;
        this.tracks = res.tracks.items;
        this.isLoading = false;
      });
    },
    setSelectedTrack(id) {
      this.selectedTrack = id;
    },
    cleanSearch() {
      this.searchQuery = "";
      this.tracks = [];
    }
  },

  computed: {
    searchMessage() {
      return `Encontrados: ${this.tracks.length}`;
    }
  },
  watch: {
    showNotification() {
      if (this.showNotification) {
        setTimeout(() => (this.showNotification = false), 3000);
      }
    }
  }
};
</script>
<style lang="scss">
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
