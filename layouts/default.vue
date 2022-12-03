<template>
  <div class="main-container center">
    <ElHeader class="main-container">
      <ElRow>
        <ElCol :span="4">
          <div class="logo-container">
            <NuxtLink to="/">
              <img
                class="logo"
                src="../assets/logo.png"
                width="45"
                height="45"
                alt="logo"
              />
            </NuxtLink>
          </div>
        </ElCol>
        <ElCol :span="20">
          <ElMenu
            :default-active="activeIndex"
            class="ElMenu-demo"
            mode="horizontal"
          >
            <ElMenu-item index="1" @click="goToHome"> Packages </ElMenu-item>
            <ElMenu-item index="2" @click="goToDaySelection"
              >Day Selection
            </ElMenu-item>
          </ElMenu>
        </ElCol>
      </ElRow>
    </ElHeader>
    <ElContainer class="container">
      <nuxt />
    </ElContainer>
    <ElFooter class="footer-container"> Copyright © 2022 MISCAN </ElFooter>
  </div>
</template>

<script>
export default {
  data() {
    return {
      activeIndex: '1',
    }
  },
  methods: {
    goToHome() {
      this.$router.push('/')
    },
    goToDaySelection() {
      this.$router.push('/day-selection')
    },
  },
}
</script>

<style >
body {
  background-color: #EDFFFD;
}
.main-container {
  min-width: 320px;
  width: 95vw;
  max-width: 1366px;
  margin: 16px auto;
}
.container {
  min-width: 300px;
  width: 100%;
  max-width: 1300px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  min-height: calc(100vh - 121px);
}
.center {
  display: grid;
  place-content: center;
}
.logo-container {
  width: 100%;
  margin: 10px auto;
  display: flex;
  justify-content: center;
  justify-items: center;
}
.logo {
  cursor: pointer;
}
.footer-container {
  display: flex;
  justify-content: center;
  justify-items: center;
  width: 100%;
  margin: 10px auto;
}
.w-full {
  width: 100%;
}
</style>
