<template>
  <div>
    <el-container id="header">
      <div class="header-top">
        <img src="../../assets/images/logo4.png" alt="" />中地基勘·勇攀高峰
      </div>
      <el-header class="header-container fixed">
        <el-menu
          @select="handleSelect"
          :default-active="activeIndex"
          mode="horizontal"
          text-color="rgba(0, 0, 0, 1)"
          active-text-color="rgba(5, 193, 211, 1)"
          background-color="rgba(255,255,255,0)"
          font-size="22px"
          class="menu"
        >
          <el-menu-item
            class="menu-item"
            v-for="(menu, index) in menus"
            :key="menu.id"
            :index="index.toString()"
          >
            {{ menu.label }}
          </el-menu-item>
        </el-menu>
      </el-header>
      <div v-if="loginStatus" class="login-status">
        <span style="cursor: pointer" class="login-span" @click="loginClick"
          >登录</span
        >
        <span
          style="
            cursor: pointer;
            height: 31px;
            line-height: 31px;
            color: rgba(0, 0, 0, 1);
          "
          >注册</span
        >
      </div>
      <main-banner
        style="position: relative; z-index: 0; background-color: #f9f9f9"
      ></main-banner>
      <div class="home-bottom">
        <img src="../../assets/images/joinus.png" alt="" />Welcome to join us
      </div>
    </el-container>
  </div>
</template>

<script>
import MainBanner from "../common/Banner";
import { getToken, setToken, removeToken } from "@/common/js/auth";

export default {
  name: "MainHeader",
  components: {
    MainBanner,
  },
  data() {
    return {
      activeIndex: "0",
      name: "0",
      user: {},
      menus: [
        {
          id: "nav1",
          label: "首页",
          path: "/website/",
        },
        {
          id: "nav2",
          label: "企业概况",
          path: "/website/Detail",
        },
        {
          id: "nav3",
          label: "全过程咨询",
          path: "/website/consultation",
        },
        {
          id: "nav4",
          label: "专家列表",
          path: "/website/professor",
        },
        {
          id: "nav5",
          label: "新闻中心",
          path: "/website/news",
        },
        {
          id: "nav6",
          label: "中地论坛",
          path: "/website/forumHome",
        },
      ],
      loginStatus: true,
      menu_style: "",
      opened: false,
    };
  },
  props: {
    currentPath: {
      type: String,
      default: "/",
    },
  },
  mounted() {
    this.menus.forEach((menu, index) => {
      if (menu.path === this.currentPath) {
        this.activeIndex = index.toString();
      }
    });
    this.verifyStatus();
  },
  computed: {
    iconClass() {
      return this.opened ? "el-icon-menu is-opened" : "el-icon-menu";
    },
  },
  methods: {
    loginClick() {
      this.$router.push({ path: "/website/login" });
    },
    iconClick() {
      this.activeIndex = "0";
      this.$router.push("/website");
    },
    verifyStatus() {
      let token = getToken();
      if (token !== undefined) {
        this.loginStatus = false;
      }
    },
    handleSelect(val) {
      debugger
      this.activeIndex = val;
      this.name = "0";
      const path = this.menus[val].path;
      const menuName = this.menus[val].label;
      if (val === "1") {
        this.$router.push({ path, query: { navType: "企业概况", navId: val } });
      } else {
        this.$router.push({ path, query: { navType: "新闻中心", navId: val } });
      }
    },
    handleChange() {
      this.opened = !this.opened;
    },
  },
};
</script>

<style lang="scss" scoped>
$layout-container-width: 900px;
$header-text-color: #005393;
.layout-container {
  width: $layout-container-width;
}
::v-deep.el-menu--horizontal > .el-menu-item {
  border-bottom: none !important;
}
.el-menu--horizontal > .el-menu-item.is-active {
  border-bottom: none !important;
}
.header-container {
  @extend .layout-container;
  min-width: 800px;
  margin-left: 300px;
}

.el-menu-item:hover {
  background-color: rgba(255, 255, 255, 0) !important;
  font-weight: bold;
}

.menu-item {
  font-family: "Microsoft YaHei";
  font-size: 16px;
}

.menu {
  text-align: center;
  display: flex;
  justify-content: space-between;
}

.login-status {
  position: absolute;
  right: 82px;
  top: 18px;
  font-size: 16px;
  font-family: "AlibabaPuHui-regular";
  z-index: 100;
  color: $header-text-color;
  width: 140px;
  display: flex;
  justify-content: space-between;
  .login-span {
    width: 91px;
    height: 31px;
    text-align: center;
    line-height: 31px;
    border-radius: 215px;
    background: rgba(5, 193, 211, 1);
    font-size: 15px;
    color: rgba(255, 255, 255, 1);
  }

  & span:hover {
    font-weight: 400;
  }
}

#mobile-header .logo {
  width: 3rem;
  height: 3rem;
}

#header .el-menu {
  border: none;
}

#mobile-navbar {
  height: 100%;
}

#mobile-navbar i {
  font-size: 1.5rem;
}
.header-top {
  position: absolute;
  display: flex;
  height: 66px;
  left: 150px;
  align-items: center;
  font-size: 16px;
  font-weight: 400;
  letter-spacing: 0px;
  line-height: 21.6px;
  color: rgba(0, 0, 0, 1);
  img {
    width: 21px;
    height: 21px;
    margin-right: 5px;
  }
}
.fixed {
  position: relative;
  height: 66px !important;
  text-align: center;
}

.menuLeft .el-menu-item:hover {
  background: rgba(255, 255, 255, 0.99) !important;
}

.menuLeft .el-submenu__title:hover {
  background: rgba(255, 255, 255, 0.99) !important;
}

.menuLeft.el-menu--horizontal > .el-menu-item {
  height: 40px;
  line-height: 40px;
}

.home-bottom {
  display: flex;
  justify-content: center;
  font-size: 16px;
  font-family: AppleSystemUIFont;
  color: #ffffff;
  text-align: center;
  width: 100%;
  height: 80px;
  background: #1d263a;
  align-items: center;
}
.home-bottom img {
  width: 24px;
  height: 24px;
  margin-right: 7px;
}
</style>
