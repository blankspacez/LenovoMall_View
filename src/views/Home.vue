<template>
  <div class="home" id="home" name="home">
    <!-- 轮播图 -->
    <div class="block">
      <el-carousel height="460px">
        <el-carousel-item v-for="item in carousel" :key="item.carouselId">
          <img style="height:460px;" :src="$target + item.imgPath" :alt="item.describes" />
        </el-carousel-item>
      </el-carousel>
    </div>
    <!-- 轮播图END -->

    <div class="main-box">
      <div class="main">

        <!-- 电脑商品展示区域 -->
        <div class="computer">
          <div class="box-hd">
            <div class="title">联想电脑</div>
          </div>
          <div class="box-bd">
            <div class="promo-list">
              <router-link to>
                <img :src="$target +'image/computer.jpg'" />
              </router-link>
            </div>
            <div class="list">
              <MyList :list="computerList" :isMore="true"></MyList>
            </div>
          </div>
        </div>
        <!-- 电脑商品展示区域END -->

        <!-- 手机商品展示区域 -->
        <div class="phone">
          <div class="box-hd">
            <div class="title">联想手机</div>
          </div>
          <div class="box-bd">
            <div class="promo-list">
              <router-link to>
                <img :src="$target +'image/phone.jpg'" />
              </router-link>
            </div>
            <div class="list">
              <MyList :list="phoneList" :isMore="true"></MyList>
            </div>
          </div>
        </div>
        <!-- 手机商品展示区域END -->

        <!-- 配件商品展示区域 -->
        <div class="accessory" id="promo-menu">
          <div class="box-hd">
            <div class="title">热门商品</div>
            <div class="more" id="more">
              <MyMenu :val="3" @fromChild="getChildMsg2">
                <span slot="1">人气热卖</span>
                <span slot="2">平板电脑</span>
                <span slot="3">充电器</span>
              </MyMenu>
            </div>
          </div>
          <div class="box-bd">
            <div class="promo-list">
              <router-link to>
                <img :src="$target +'image/pad.jpg'" />
              </router-link>
<!--              <ul>-->
<!--                <li>-->
<!--                  <img :src="$target +'image/pad.jpg'" alt />-->
<!--                </li>-->
<!--                <li>-->
<!--                  <img :src="$target +'public/imgs/accessory/accessory-promo2.png'" alt />-->
<!--                </li>-->
<!--              </ul>-->
            </div>
            <div class="list">
              <MyList :list="accessoryList" :isMore="true"></MyList>
            </div>
          </div>
        </div>
        <!-- 配件商品展示区域END -->
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      carousel: "", // 轮播图数据
      computerList: "", // 联想电脑商品列表
      phoneList: "", // 手机商品列表
      // applianceList: "", // 家电商品列表
      // applianceHotList: "", //热门家电商品列表
      accessoryList: "", //配件商品列表
      accessoryHotList: "", //热门配件商品列表
      //protectingShellList: "", // 保护套商品列表
      padList:"", //平板电脑商品列表
      chargerList: "", //充电器商品列表
      //applianceActive: 1, // 家电当前选中的商品分类
      accessoryActive: 1 // 配件当前选中的商品分类
    };
  },
  watch: {
    // // 家电当前选中的商品分类，响应不同的商品数据
    // applianceActive: function(val) {
    //   // 页面初始化的时候把applianceHotList(热门家电商品列表)直接赋值给applianceList(家电商品列表)
    //   // 所以在切换商品列表时判断applianceHotList是否为空,为空则是第一次切换,把applianceList赋值给applianceHotList
    //   if (this.applianceHotList == "") {
    //     this.applianceHotList = this.applianceList;
    //   }
    //   if (val == 1) {
    //     // 1为热门商品
    //     this.applianceList = this.applianceHotList;
    //     return;
    //   }
    //   if (val == 2) {
    //     // 2为电视商品
    //     this.applianceList = this.miTvList;
    //     return;
    //   }
    // },
    accessoryActive: function(val) {
      // 页面初始化的时候把accessoryHotList(热门配件商品列表)直接赋值给accessoryList(配件商品列表)
      // 所以在切换商品列表时判断accessoryHotList是否为空,为空则是第一次切换,把accessoryList赋值给accessoryHotList
      if (this.accessoryHotList == "") {
        this.accessoryHotList = this.accessoryList;
      }
      if (val == 1) {
        // 1为热门商品
        this.accessoryList = this.accessoryHotList;
        return;
      }
      if (val == 2) {
        // 2为平板电脑商品
        this.accessoryList = this.padList;
        return;
      }
      if (val == 3) {
        // 3为充电器商品
        this.accessoryList = this.chargerList;
        return;
      }
    }
  },
  created() {
    // 获取轮播图数据
    this.$axios
      .get("/api/resources/carousel", {})
      .then(res => {
        this.carousel = res.data.data;
      })
      .catch(err => {
        return Promise.reject(err);
      });
    // 获取各类商品数据
    this.getPromo(2, "computerList");
    this.getPromo(1, "phoneList");
    this.getPromo(3, "padList");
    this.getPromo(5, "chargerList");
    // this.getHot(
    //   "applianceList"
    // );
    this.getHot(
       "accessoryList"
    );
  },
  methods: {
    // 获取家电模块子组件传过来的数据
    // getChildMsg(val) {
    //   this.applianceActive = val;
    // },
    // 获取配件模块子组件传过来的数据
    getChildMsg2(val) {
      this.accessoryActive = val;
    },
    // 获取各类商品数据方法封装
    getPromo(categoryId, val) {
      let api = "/api/product/category/limit/" + categoryId;
      this.$axios
        .get(api)
        .then(res => {
          this[val] = res.data.data;
        })
        .catch(err => {
          return Promise.reject(err);
        });
    },
    getHot(val) {
      let api = "/api/product/category/hot";
      this.$axios
        .get(api)
        .then(res => {
          this[val] = res.data.data;
        })
        .catch(err => {
          return Promise.reject(err);
        });
    }
  }
};
</script>
<style scoped>
@import "../assets/css/index.css";
menu-right {
  position: fixed;
  top: 10%;
  right: 0;
  z-index: 999;
}
.menu-right .item {
  width: 84px;
  height: 92px;
  background: white;
  border-bottom: 1px solid #f5f5f5;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.menu-right img {
  width: 60px;
  height: 60px;
  -webkit-filter: grayscale(100%);
  -moz-filter: grayscale(100%);
  -o-filter: grayscale(100%);
  filter: grayscale(100%);
}
.menu-right .text {
  color: #757575;
  margin-top: 10px;
}
</style>