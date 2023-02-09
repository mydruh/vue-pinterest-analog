<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <header>
  <div class="navbar navbar-black bg-black shadow-sm">
    <div class="container">
      <RouterLink to="/" class="navbar-brand d-flex align-items-center logo-nav">
        <img class="logo-nav" src="./assets/logoArt.png"/>
        <!-- <strong class="logo-art">ART</strong>
        <strong>GALERY</strong>
        <i class="bi bi-brush"></i> -->
      </RouterLink>
      <RouterLink to="/favorites" style="color: white" class="navbar-brand d-flex align-items-center logo-nav">
        <i class="bi bi-heart" style="margin-right: 10px;"></i> Избранное
      </RouterLink>
    </div>
  </div>
</header>

  <RouterView />
</template>

<style scoped>
</style>
