<template lang="pug">
  .card.card-vue(v-if="track && track.album")
    .card-image
      .figure.image.is-1by1
        img(:src="track.album.images[0].url")
    .card-content
      .media
        .media-left(@click="selectTrack")
          figure.image.is-48x48
            img(:src="track.album.images[0].url")
        .media-content
          p.title.is-6
            strong {{track.name}}
          p.subtitle.is-6 {{track.artists[0].name}}

      .content
        small {{track.duration_ms | ms-to-mm}}
        nav.level
          .level-left
            button.button.level-item
              span.fas.fa-play(@click="selectTrack")
            button.button.level-item
              span.fas.fa-expand(@click="goToTrack(track.id)")
</template>

<script>
import trackMixin from "@/mixins/mixin.track";

export default {
  mixins: [trackMixin],
  props: {
    track: {
      type: Object,
      required: true
    }
  },
  methods: {
    goToTrack(id) {
      if (!this.track.preview_url) {
        return;
      }
      this.$router.push({ name: "track", params: { id: id } });
    }
  }
};
</script>
<style scoped>
.card {
  padding: 5px;
  color: black !important;
}
.card:hover {
  box-shadow: 0 2px 3px rgba(10, 10, 10, 0.2), 0 0 0 1px rgba(10, 10, 10, 0.2);
}
</style>
