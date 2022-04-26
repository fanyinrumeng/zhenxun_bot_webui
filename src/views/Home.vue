<template>
  <div>
    <el-container>
      <el-header class="homeHeader">
        <div class="title">小真寻的后台捏</div>
      </el-header>
      <el-container class="layoutbox">
        <el-aside :width="isCollapse ? '64px' : '150px'" class="left-aside">
          <div class="toggle-button" @click="toggleCollapse()">|||</div>
          <el-scrollbar class="myscrollbar">
            <el-menu @select="handleSelect" :collapse="isCollapse" :collapse-transition="false">
              <el-menu-item index="/plugin">
                <i class="el-icon-menu"></i>
                <span slot="title">插件列表</span>
              </el-menu-item>
              <el-menu-item index="/group">
                <i class="el-icon-menu"></i>
                <span slot="title">群组管理</span>
              </el-menu-item>
              <el-menu-item index="/request">
                <i class="el-icon-menu"></i>
                <span slot="title">请求管理</span>
              </el-menu-item>
              <el-menu-item index="/system">
                <i class="el-icon-menu"></i>
                <span slot="title">系统信息</span>
              </el-menu-item>
            </el-menu>
          </el-scrollbar>
        </el-aside>
        <el-main><router-view class="homeRouterView" /></el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script>
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Home",
  data() {
    return { isCollapse: false };
  },
  methods: {
    handleSelect(index) {
      this.$router.replace(index);
    },
    toggleCollapse() {this.isCollapse = !this.isCollapse;},
  },
};
</script>

<style>
.homeHeader {
  background-color: #409eff;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0px 15px;
  box-sizing: border-box;
}

.homeHeader .title {
  font-size: 30px;
  font-family: 宋体;
  color: #ffffff;
}

.left-aside {
  background-color: #ffffff;
  overflow-y: hidden;
}

.toggle-button {
  font-size: 10px;
  line-height: 24px;
  cursor: pointer;
}

.myscrollbar {
  height: 100%;
  text-align: center;
}
.myscrollbar >>> .el-scrollbar__wrap {
  overflow-x: hidden;
}
.layoutbox {
  height: 85vh;
  background-color: #f0f2f5;
}
</style>
