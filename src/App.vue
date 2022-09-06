<template>
  <getUserInfo />
</template>

<script>
import getUserInfo from "./components/getUserInfo.vue";

export default {
  name: "App",
  components: {
    getUserInfo,
  },
};
</script>

<style lang="less">
body {
  min-height: 100vh;
  overflow: hidden;
  background: rgb(128, 128, 128);
}

div,
body {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
