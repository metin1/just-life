<template>
  <div class="main-container center">
    <ElHeader class="main-container">
      <ElRow>
        <ElCol :span="8">
          <NuxtLink to="/">
          <div class="logo-container">
              <img
                class="logo"
                src="../assets/logo.png"
                width="45"
                height="45"
                alt="logo"
              />
              <div class="logo-text">
                JUST LIFE
              </div>
            </div>
          </NuxtLink>
        </ElCol>
        <ElCol :span="16">
          <ElMenu
            :default-active="activeIndex"
            class="nav-menu"
            background-color="var(--background-color)"
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
  watch: {
    $route(to, from) {
      if (to.path === '/') {
        this.activeIndex = '1'
      } else if (to.path === '/day-selection') {
        this.activeIndex = '2'
      }
    },
  },

}
</script>

<style >
:root {
  --dark-color: #386641;
  --primary-color: #6A994E;
  --light-color: #c0d888;
  /* --light-color: #A7C957; */
  /* --light-color: #a5e887; */
  --secondary-color: #BC4749;
  --danger-color: #ffbfbf;
  --background-color: #F2E8CF;
}
body {
  background-color: var(--background-color);
  font-family: sans-serif;
  color: var(--dark-color);
}
h1 {
  font-size: 2.5rem;
  font-weight: 700;
}
h2 {
  font-size: 2rem;
  font-weight: 700;
}
h3 {
  font-size: 1.5rem;
  font-weight: 700;
}
h4 {
  font-size: 1.25rem;
  font-weight: 700;
}

a {
  color: var(--dark-color);
  text-decoration: none;
}
.nav-menu {
  background-color: var(--background-color);
  font: 1.4rem;
  font-weight: 500;
}
.main-container {
  min-width: 320px;
  width: 95vw;
  max-width: 1366px;
  margin: 0px auto;
}
.container {
  min-width: 300px;
  width: 100%;
  max-width: 1300px;
  display: flex;
  justify-content: center;
  min-height: calc(100vh - 136px);
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
  align-items: center;
}
.logo {
  cursor: pointer;
}
.footer-container {
  display: flex;
  justify-content: center;
  justify-items: center;
  width: 100%;
  margin: 0px auto;
}
.w-full {
  width: 100%;
}
.logo-text {
  font-size: 1.5rem;
  font-weight: 700;
  margin-left: 10px;
}
</style>
