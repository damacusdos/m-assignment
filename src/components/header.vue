<template>
  <div class="header-container">
    <div class="logo-container">
      <img src="../assets/images/default.png" alt="">
    </div>
    <div class="title-container">
      <div class="home-logo">
        <img src="../assets/images/home-button.png" alt="">
      </div>
      <div class="home-text">
        INVENTORY
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {

    }
  }
  
}
</script>

<style scoped>
.header-container {
  width: 100vw;
  display: flex;
  justify-content: flex-start;
  background-color:#E31E30;
  height: 65px;
;
}
.logo-contaienr {
  width: 190px;
}
.title-container {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 176px;
  border-bottom: 2px solid #fff;
}
.home-logo {
  width: 16px;
}
.home-text {
  color: #fff;
  margin-left: 16px;
}
</style>