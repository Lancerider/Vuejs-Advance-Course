<template lang="pug">
  .container
    .columns
      .column.is-5.is-offset-4
        .notification.is-danger
          slot(name="body") Al parecer hubo un error...
</template>

<style lang="scss" scoped>
.notification {
  margin: 10px;
}
</style>
