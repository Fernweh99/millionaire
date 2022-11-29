<template>
  <div class="home">
    <div class="container">
      <div>
        <img id="logo" class="mb-4" src="../assets/image/logo.png" alt="" />
        <h2 class="mb-5">Sei pronto a scalare la classifica e diventare milionario?</h2>
        <router-link :to="{ name: 'game' }">
          <ContainerComponent text="Nuova Partita" />
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
import ContainerComponent from "@/components/ContainerComponent.vue";
export default {
  name: "HomeView",
  components: {
    ContainerComponent,
  },
};
</script>

<style lang="scss" scoped>
.home{
  color: white;
  text-align: center;
  padding: 50px 0;
  a {
    text-decoration: none;
    color: white;
  }#logo {
    height: 300px;
  }
}
</style>
