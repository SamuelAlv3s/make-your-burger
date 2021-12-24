<template>
  <footer id="footer">
    <p>Make Your Burguer &copy; 2021</p>
  </footer>
</template>
<script>
export default {
  name: "Footer",
};
</script>
<style scoped>
#footer {
  width: 100%;
  height: 150px;
  background-color: #222;
  border-top: 4px solid #111;
  color: #fcba03;
  display: flex;
  justify-content: center;
  align-items: center;
  position: fixed;
  bottom: 0;
}
</style>
