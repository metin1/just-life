<template>
  <div class="center">
    <el-header>
      <el-row>
        <el-col :span="4">
          <div class="logo-container">
            <img
              class="center"
              src="../assets/logo.png"
              width="45"
              height="45"
              alt="logo"
            />
          </div>
        </el-col>
        <el-col :span="20">
          <el-menu
            :default-active="activeIndex"
            class="el-menu-demo"
            mode="horizontal"
          >
            <el-menu-item index="1"
              ><nuxt-link to="/">Packages</nuxt-link>
            </el-menu-item>
            <el-menu-item index="2"
              ><nuxt-link to="/day-selection">Day Selection</nuxt-link>
            </el-menu-item>
          </el-menu>
        </el-col>
      </el-row>
    </el-header>
    <el-container>
      <nuxt />
    </el-container>
    <el-footer class="footer-container "> Copyright © 2022 MISCAN </el-footer>
  </div>
</template>

<script>
export default {
  data() {
    return {
      activeIndex: '1',
    }
  },
}
</script>

<style scoped>
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
.footer-container {
  display: flex;
  justify-content: center;
  justify-items: center;
  width: 100%;
  margin: 10px auto;
}
</style>
