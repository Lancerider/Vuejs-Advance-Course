<template lang="pug">
#app
  PmHeader

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
            :class="{'is-active':track.id===selectedTrack}"
            v-bind:track="track",
            @select="setSelectedTrack"
            )

  PmFooter
</template>


<script>
import trackService from "@/services/track";
import PmFooter from "@/components/layout/Footer.vue";
import PmHeader from "@/components/layout/Header.vue";

import PmTrack from "@/components/Track.vue";
import PmLoader from "@/components/shared/Loader.vue";

export default {
  name: "app",

  components: { PmFooter, PmHeader, PmTrack, PmLoader },

  data() {
    return {
      searchQuery: "",
      tracks: [],

      isLoading: false,

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
    },
    pressedKey: function(evt) {
      if (evt.keyCode === 27) {
        this.cleanSearch();
      }
    }
  },

  computed: {
    searchMessage() {
      return `Encontrados: ${this.tracks.length}`;
    }
  },
  created: function() {
    document.addEventListener("keyup", this.pressedKey);
  },
  destroyed: function() {
    document.removeEventListener("keyup", this.pressedKey);
  }
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
