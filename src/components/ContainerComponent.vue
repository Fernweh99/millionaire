<template>
  <div
    class="my_container"
    :style="size == 'xl' ? 'max-width: 600px; min-width: 500px' : ''"
  >
    <span>{{ text }}</span>
  </div>
</template>

<script>
export default {
  name: "ContainerComponent",
  props: {
    text: String,
    size: String,
  },
};
</script>

<style lang="scss" scoped>
.my_container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 70px;
  min-width: 300px;
  max-width: 300px;
  border: 2px solid white;
  border-radius: 50px;
  color: white;
  margin: 15px auto;
}
</style>
