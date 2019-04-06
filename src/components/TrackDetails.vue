<template lang="pug">
.container
  .columns
    .column.is-3.has-text-centered
      figure.media-left
        p.image
          img(:src="track.album.images[0].url")
        p


    .column.is-8
      .panel
        .panel-heading
          h1.title {{track.name}}
          a.button.is-primary.is-large
            span.fas.fa-play(@click="selectTrack")
        .panel-block
          article.media
            .media-content
              .content
                ul(v-for="(value,key) in track")
                  li
                    strong {{ key }}:&nbsp;
                    span {{ value}}

                  nav.level
                    .level-left
                      a.level-item
</template>

<script>
import trackService from "@/services/track";
import trackMixin from "@/mixins/mixin.track";

export default {
  mixins: [trackMixin],
  data() {
    return {
      track: {}
    };
  },
  created() {
    const id = this.$route.params.id;

    trackService.getById(id).then(res => {
      this.track = res;
    });
  }
};
</script>

<style lang="scss" scoped>
.columns {
  margin: 20px;
}
</style>
