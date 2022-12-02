<template>
  <Transition name="fade" mode="out-in">
    <router-view />
  </Transition>
</template>

<style lang="scss">
@import "~bootstrap/scss/bootstrap.scss";
body {
  background-color: #120939;
  .fade-enter-active,
  .fade-leave-active {
    transition: opacity 0.3s linear;
  }
  .fade-enter-from,
  .fade-leave-to {
    opacity: 0;
  }
}
</style>
