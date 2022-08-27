<template>
  <div id="app" name="app">
    <el-container>
      <!-- 顶部导航栏 -->
      <div class="topbar">
        <div class="nav">
          <ul>
            <li>
              <router-link to="/order">我的订单</router-link>
            </li>
            <li>
              <router-link to="/collect">我的收藏</router-link>
            </li>
            <li :class="getNum > 0 ? 'shopCart-full' : 'shopCart'">
              <router-link to="/shoppingCart">
                <i class="el-icon-shopping-cart-full"></i> 购物车
                <span class="num">({{ getNum }})</span>
              </router-link>
            </li>
            <li v-if="!this.$store.getters.getUser">
              <el-button type="text" @click="register = true">注册</el-button>
              <span class="sep">|</span>
              <el-button type="text" @click="login">登录</el-button>
            </li>
            <li v-else>
              欢迎
              <el-popover placement="top" width="180" v-model="visible">
                <p>确定退出登录吗？</p>
                <div style="text-align: right; margin: 10px 0 0">
                  <el-button size="mini" type="text" @click="visible = false"
                  >取消</el-button
                  >
                  <el-button type="primary" size="mini" @click="logout"
                  >确定</el-button
                  >
                </div>
                <el-button type="text" slot="reference">{{
                    this.$store.getters.getUser.username
                  }}</el-button>
              </el-popover>
            </li>
          </ul>
        </div>
      </div>
      <!-- 顶部导航栏END -->

      <!-- 顶栏容器 -->
      <el-header>
        <el-menu
            :default-active="activeIndex"
            class="el-menu-demo"
            mode="horizontal"
            active-text-color="#ff0d0d"
            router
        >
          <div class="logo">
            <router-link to="/">
              <img src="./assets/imgs/logo.jpg" width="90%" alt />
            </router-link>
          </div>
          <el-menu-item index="/">首页</el-menu-item>
          <el-menu-item index="/goods">全部商品</el-menu-item>
          <el-menu-item index="/seckill">秒杀商品</el-menu-item>
          <el-menu-item index="/about">关于我们</el-menu-item>
          <!-- <el-menu-item index="/about">关于我们</el-menu-item> -->

          <div class="so">
            <el-input placeholder="请输入搜索内容" v-model="search">
              <el-button
                  slot="prepend"
                  icon="el-icon-search"
                  @click="searchClick"
              ></el-button>
            </el-input>
          </div>
        </el-menu>
      </el-header>
      <!-- 顶栏容器END -->

      <!-- 登录模块 -->
      <MyLogin></MyLogin>
      <!-- 注册模块 -->
      <MyRegister :register="register" @fromChild="isRegister"></MyRegister>

      <!-- 主要区域容器 -->
      <el-main>
        <keep-alive>
          <router-view></router-view>
        </keep-alive>
      </el-main>
      <!-- 主要区域容器END -->

      <!-- 底栏容器 -->
      <el-footer>
        <div class="footer">
          <div class="ng-promise-box">
            <div class="ng-promise">
              <!--<p class="text">
                <a class="icon1" href="javascript:;">7天无理由退换货</a>
                <a class="icon2" href="javascript:;">满99元全场免邮</a>
                <a class="icon3" style="margin-right: 0" href="javascript:;">100%品质保证</a>
              </p>-->
              <div class="container yejiao-1">
                <img
                    src="./assets/imgs/bottom.png"
                    width="100%"
                />
              </div>
            </div>
          </div>
          <!--<div class="github">
            <a href="https://github.com/ZeroWdd" target="_blank">
              <div class="github-but"></div>
            </a>
          </div>-->
          <div class="container po">
            <div class="container ye">
              <div class="yejiao-2">
                <div class="te-1">购物指南</div>
                <div class="kong"></div>
                <div class="te-2">服务商信息</div>
                <div class="te-2">购买流程</div>
                <div class="te-2">注册登录</div>
                <div class="te-2">商品评价</div>
              </div>

              <div class="yejiao-2">
                <div class="te-1">配送方式</div>
                <div class="kong"></div>
                <div class="te-2">配送方式</div>
                <div class="te-2">配送方式信息</div>
                <div class="te-2">签收原则</div>
                <div class="te-2">物流查询</div>
              </div>

              <div class="yejiao-2">
                <div class="te-1">支付方式</div>
                <div class="kong"></div>
                <div class="te-2">分期支付</div>
                <div class="te-2">支付方式</div>
                <div class="te-2">支付问题</div>
              </div>

              <div class="yejiao-2">
                <div class="te-1">订单信息</div>
                <div class="kong"></div>
                <div class="te-2">订单信息</div>
                <div class="te-2">发票信息</div>
                <div class="te-2">手机激活查询</div>
              </div>

              <div class="yejiao-2">
                <div class="te-1">售后服务</div>
                <div class="kong"></div>
                <div class="te-2">售后服务总则</div>
                <div class="te-2">24小时在线客服</div>
                <div class="te-2">联想服务入口</div>
                <div class="te-2">热门解决方案</div>
                <div class="te-2">Lenovo Quick Fix工具</div>
              </div>

              <div class="yejiao-2">
                <div class="te-1">其他说明</div>
                <div class="kong"></div>
                <div class="te-2">服务承诺</div>
                <div class="te-2">账户安全</div>
                <div class="te-2">手机产品购买须知</div>
                <div class="te-2">环境信息</div>
              </div>

              <div class="yejiao-c">
                <div><h3>400-166-6666</h3></div>
                <div class="te-2">周一到周日 9:00-21:00</div>
<!--                <div>-->
<!--                  <input type="" name="" value="在线咨询" class="tet" />-->
<!--                </div>-->
              </div>
            </div>
          </div>
          <div class="mod_help">
            <p>
              <router-link to="/">首页</router-link>
              <span>|</span>
              <router-link to="/goods">全部商品</router-link>
              <span>|</span>
              <router-link to="/seckill">秒杀商品</router-link>
              <span>|</span>
              <router-link to="/about">关于我们</router-link>
            </p>
            <p class="coty">商城版权所有 &copy; 2022</p>
          </div>
        </div>
      </el-footer>
      <!-- 底栏容器END -->
    </el-container>
  </div>
</template>

<script>
import { mapActions } from "vuex";
import { mapGetters } from "vuex";

export default {
  beforeUpdate() {
    this.activeIndex = this.$route.path;
  },
  data() {
    return {
      activeIndex: "", // 头部导航栏选中的标签
      search: "", // 搜索条件
      register: false, // 是否显示注册组件
      visible: false, // 是否退出登录
    };
  },
  created() {
    // 对用户信息进行校验，并刷新cookie
    if (this.getCookie("XM_TOKEN") != null) {
      this.$axios.get("/api/user/token").then((res) => {
        if (res.data.code === "001") {
          // 001 为成功
          this.setUser(res.data.data);
        }
      });
      let cookie = this.getCookie("XM_TOKEN");
      let user = {};
      user.userId = cookie.split("|")[1];
      user.username = cookie.split("|")[2];
      this.setUser(user);
    }

    // window.setTimeout(() => {
    //   this.$message({
    //     duration: 0,
    //     showClose: true,
    //     message: `
    //     <p>如果觉得这个项目还不错，</p>
    //     <p style="padding:10px 0">您可以给项目源代码仓库点Star支持一下，谢谢！</p>
    //     <p><a href="https://github.com/hai-27/vue-store" target="_blank">Github传送门</a></p>`,
    //     dangerouslyUseHTMLString: true,
    //     type: "success"
    //   });
    // }, 1000 * 60);
  },
  computed: {
    ...mapGetters(["getUser", "getNum"]),
  },
  watch: {
    // 获取vuex的登录状态
    getUser: function (val) {
      if (val === "") {
        // 用户没有登录
        this.setShoppingCart([]);
      } else {
        // 用户已经登录,获取该用户的购物车信息
        this.$axios
            .get("/api/cart/user/" + val.userId)
            .then((res) => {
              if (res.data.code === "001") {
                // 001 为成功, 更新vuex购物车状态
                this.setShoppingCart(res.data.data);
              } else {
                // 提示失败信息
                this.notifyError(res.data.msg);
              }
            })
            .catch((err) => {
              return Promise.reject(err);
            });
      }
    },
  },
  methods: {
    ...mapActions(["setUser", "setShowLogin", "setShoppingCart"]),
    login() {
      // 点击登录按钮, 通过更改vuex的showLogin值显示登录组件
      this.setShowLogin(true);
    },
    // 退出登录
    logout() {
      this.visible = false;
      // 清空cookie中的token
      this.delCookie("XM_TOKEN");
      // 清空vuex登录信息
      this.setUser("");
      this.notifySucceed("成功退出登录");
    },
    // 接收注册子组件传过来的数据
    isRegister(val) {
      this.register = val;
    },
    // 点击搜索按钮
    searchClick() {
      if (this.search != "") {
        // 跳转到全部商品页面,并传递搜索条件
        this.$router.push({ path: "/goods", query: { search: this.search } });
        this.search = "";
      }
    },
    getCookie(name) {
      //获取指定名称的cookie值
      // (^| )name=([^;]*)(;|$),match[0]为与整个正则表达式匹配的字符串，match[i]为正则表达式捕获数组相匹配的数组；
      let arr = document.cookie.match(
          new RegExp("(^| )" + name + "=([^;]*)(;|$)")
      );
      if (arr != null) {
        return unescape(arr[2]);
      }
      return null;
    },
    delCookie(name) {
      let exp = new Date();
      exp.setTime(exp.getTime() - 1);
      let cval = this.getCookie(name);
      if (cval != null)
        document.cookie = name + "=" + cval + ";expires=" + exp.toGMTString();
    },
  },
};
</script>

<style>
/* 全局CSS */
* {
  padding: 0;
  margin: 0;
  border: 0;
  list-style: none;
}
#app .el-header {
  padding: 0;
}
#app .el-main {
  min-height: 300px;
  padding: 20px 0;
}
#app .el-footer {
  padding: 0;
}
a,
a:hover {
  text-decoration: none;
}
/* 全局CSS END */

/* 顶部导航栏CSS */
.topbar {
  height: 40px;
  background-color: #3d3d3d;
  margin-bottom: 20px;
}
.topbar .nav {
  width: 1225px;
  margin: 0 auto;
}
.topbar .nav ul {
  float: right;
}
.topbar .nav li {
  float: left;
  height: 40px;
  color: #b0b0b0;
  font-size: 14px;
  text-align: center;
  line-height: 40px;
  margin-left: 20px;
}
.topbar .nav .sep {
  color: #b0b0b0;
  font-size: 12px;
  margin: 0 5px;
}
.topbar .nav li .el-button {
  color: #b0b0b0;
}
.topbar .nav .el-button:hover {
  color: #fff;
}
.topbar .nav li a {
  color: #b0b0b0;
}
.topbar .nav a:hover {
  color: #fff;
}
.topbar .nav .shopCart {
  width: 120px;
  background: #424242;
}
.topbar .nav .shopCart:hover {
  background: #fff;
}
.topbar .nav .shopCart:hover a {
  color: #ff6700;
}
.topbar .nav .shopCart-full {
  width: 120px;
  background: #ff6700;
}
.topbar .nav .shopCart-full a {
  color: white;
}
/* 顶部导航栏CSS END */

/* 顶栏容器CSS */
.el-header .el-menu {
  max-width: 1225px;
  margin: 0 auto;
}
.el-header .logo {
  height: 60px;
  width: 200px;
  float: left;
  margin-top: 15px;
  margin-right: 80px;
}
.el-header .so {
  margin-top: 10px;
  width: 300px;
  float: right;
}
/* 顶栏容器CSS END */

/* 底栏容器CSS */
.footer {
  width: 100%;
  text-align: center;
  background: #000000;
  padding-bottom: 20px;
}
.footer .ng-promise-box {
  border-bottom: 1px solid #b4b1b1;
  line-height: 145px;
}
.footer .ng-promise-box {
  margin: 0 auto;
  border-bottom: 1px solid #272727;
  line-height: 145px;
}
.footer .ng-promise-box .ng-promise p a {
  color: #fff;
  font-size: 20px;
  margin-right: 210px;
  padding-left: 44px;
  height: 40px;
  display: inline-block;
  line-height: 40px;
  text-decoration: none;
  background: url("./assets/imgs/us-icon.png") no-repeat left 0;
}
.footer .github {
  height: 50px;
  line-height: 50px;
  margin-top: 20px;
}
.footer .github .github-but {
  width: 50px;
  height: 50px;
  margin: 0 auto;
  background: url("./assets/imgs/github.png") no-repeat;
}
.footer .mod_help {
  text-align: center;
  color: #888888;
}
.footer .mod_help p {
  margin: 20px 0 16px 0;
}

.footer .mod_help p a {
  color: #888888;
  text-decoration: none;
}
.footer .mod_help p a:hover {
  color: #fff;
}
.footer .mod_help p span {
  padding: 0 22px;
}

.container {
  text-align: center;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  background: #f1f0e9;
}
.yejiao-1 {
  width: 100%;
  height: 199px;
  background: white;
  justify-content: center;
  position: relative;
}
.yejiao-1 img {
  width: 80%;
  height: 199px;
}

.yejiao-2 {
  display: flex;
  flex-direction: column;
  width: 10%;
  height: 200px;
  line-height: 20px;
  position: relative;
  left: 200px;
}
.yejiao-c {
  display: flex;
  flex-direction: column;
  width: 20%;
  height: 200px;
  line-height: 20px;
  position: relative;
  left: 200px;
}
.kong {
  width: 10%;
  height: 10px;
}
.te-1 {
  font-size: 16px;
  font-family: 微软雅黑;
  color: #1f1f1f;
  font-weight: 600;
}
.te-2 {
  font-size: 13px;
  font-family: 微软雅黑;
  color: #757575;
}
.ye {
  width: 70%;
  height: 298px;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}
.tet {
  text-align: center;
  font-size: 17px;
  font-family: 微软雅黑;
  color: #757575;
  font-weight: 800;
}

.yejiao-bg {
  background: black;
  width: 100%;
  height: 46px;
  display: flex;
  justify-content: center;
}
.yejiao-word {
  font-size: 12px;
  font-family: 微软雅黑;
  color: #757575;
  height: 46px;
  display: flex;
  justify-content: space-between;
  width: 80%;
}
.po {
  cursor: default;
}
.po:hover {
  cursor: pointer;
}
/* 底栏容器CSS END */
</style>