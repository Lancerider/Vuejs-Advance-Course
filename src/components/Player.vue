<template lang="pug">
  .content(v-if="track && track.album")
    p.is-128x128
      img.is-128x128(:src="track.album.images[0].url")
    p.has-text-centered
      strong.has-text-white {{ track.name}}
    p.has-text-white.has-text-centered [{{ track.duration_ms | ms-to-mm}}]
    p
      audio(controls,autoplay, :src="track.preview_url")
</template>

<script>
import { mapState } from "vuex";
export default {
  computed: {
    ...mapState(["track"]),
    thereIsATrack() {
      return track != {} ? true : false;
    }
  }
};
</script>

<style scoped>
</style>
