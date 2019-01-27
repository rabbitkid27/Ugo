<template>
  <div class="index-container">
    <!-- 搜索框区域 -->
    <div class="search-box">
      <input type="text" placeholder="搜索">
      <icon type="search" size="12"></icon>
    </div>
    <!-- 轮播图区域 -->
    <div class="swiper-container">
      <swiper circular indicator-dots autoplay indicator-active-color="#0094ff">
        <swiper-item v-for="item in swiperList" :key="item.image_src">
          <img mode="aspectFill" :src="item.image_src">
        </swiper-item>
      </swiper>
    </div>
    <!-- 分类按钮区域 -->
    <div class="category-container">
      <div class="item" v-for="item in categoryList">
        <img :src="item.image_src">
        <p>{{item.name}}</p>
      </div>
    </div>
    <!-- 楼层区域 -->
    <div class="floor-container">
      <div class="floor" v-for="item in floorList">
        <!-- 顶部 -->
        <div class="top">
          <img :src="item.floor_title.image_src">
          <h3>{{item.floor_title.name}}</h3>
        </div>
        <!-- 底部 -->
        <div class="bottom">
          <img v-for="(it, i) in item.product_list" :src="it.image_src">
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import hxios from '../../utils/index.js'

export default {
  data() {
    return {
      // 轮播图数组
      swiperList: [],
      // 分类按钮数组
      categoryList: [],
      // 楼层区域数组
      floorList: []
    }
  },
  // 初始化的时候 获取数据
  async created() {
    // 方法 - ***壹*** - ! 逐个数据获取 :
    // 轮播图的数据
    // let swiperRes = await hxios.get({
    //   url: "api/public/v1/home/swiperdata"
    // });
    // // console.log(swiperRes);
    // this.swiperList = swiperRes.data.message;

    // // 分类按钮的数据
    // let categoryRes=await hxios.get({
    //   url:"api/public/v1/home/catitems"
    // });
    // this.categoryList=categoryRes.data.message;

    // //获取楼层数据
    // let floorRes=await hxios.get({
    //   url:"api/public/v1/home/floordata"
    // });
    // this.floorList=floorRes.data.message;

    // 方法 - ***贰*** - ! 一次性获取3个 :
    let p1 = hxios.get({
      url: 'api/public/v1/home/swiperdata'
    })
    let p2 = hxios.get({
      url: 'api/public/v1/home/catitems'
    })
    let p3 = hxios.get({
      url: 'api/public/v1/home/floordata'
    })
    // 一次性发送所有的请求
    let totalRes=await Promise.all([p1,p2,p3]);
    
    this.swiperList = totalRes[0].data.message;
    this.categoryList = totalRes[1].data.message;
    this.floorList = totalRes[2].data.message;
  }
}
</script>

<style lang="scss">
$uRed: #ff2d4a;
.index-container {
  padding-top: 100rpx;
}
.search-box {
  background-color: $uRed;
  padding: 20rpx 16rpx;
  box-sizing: border-box;
  left: 0;
  top: 0;
  position: fixed;
  width: 100%;
  input {
    display: block;
    width: 100%;
    box-sizing: border-box;
    padding-left: 376rpx;
    background-color: #ffffff;
    font-size: 24rpx;
    height: 60rpx;
    border-radius: 10rpx;
  }
  icon {
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
  }
}
.swiper-container {
  swiper {
    height: 340rpx;
    swiper-item {
      height: 100%;
    }
  }
  img {
    display: block;
    width: 100%;
    height: 100%;
  }
}
// 分类按钮区域
.category-container {
  display: flex;
  padding-top: 24px;
  padding: 29rpx;
  background-color: white;
  .item {
    flex: 1;
    img {
      display: block;
      width: 128rpx;
      height: 128rpx;
      margin: 0 auto;
    }
    p {
      text-align: center;
      margin-top: 10rpx;
      font-size: 24rpx;
    }
  }
}
// 楼层区域
.floor-container {
  .floor {
    .top {
      padding-top: 30rpx;
      padding-bottom: 30rpx;
      padding-left: 15rpx;
      position: relative;
      height: 90rpx;
      box-sizing: border-box;
      h3 {
        color: #ff7b94;
        position: absolute;
        left: 30rpx;
        top: 50%;
        transform: translateY(-50%);
      }
      img {
        position: absolute;
        height: 90rpx;
        left: 0;
        top: 0;
        display: block;
        width: 100%;
      }     
    }
    .bottom {
      padding: 20rpx 0 0 16rpx;
      overflow: hidden;
      img {
        display: block;
        width: 33.333%;
        float: left;
        width: 230rpx;
        height: 190rpx;
        margin-right: 10rpx;
        &:first-child{
          width: 230rpx;
          height: 390rpx;
        }
        &:nth-child(2){
          margin-bottom: 10rpx;
        }
        &:nth-child(3){
          margin-bottom: 10rpx;
        }
      }
    }
  }
}
</style>
