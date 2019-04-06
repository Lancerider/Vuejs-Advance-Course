<template lang="pug">
  section.hero.cs-dark
    .hero-head
      header.nav
        .container
          .nav-left
            .nav-item
              strong 📻  Platzi Music
            .nav-right.nav-menu
              router-link.nav-item(to="search") Buscar
              router-link.nav-item(to="about") Nosotros
    .hero-body
      .container.has-text-centered
        h1.title.has-text-white Platzi Music
        h2.subtitle.has-text-white Songs for you
        Pm-Player.size
</template>

<script>
import PmPlayer from "@/components/Player.vue";

export default {
  components: { PmPlayer }
};
</script>

<style>
.hero-head {
  background-color: #fff;
}
.cs-dark {
  background: #1e2656;
}
.size {
  width: 350px;
  margin: auto;
}
</style>
