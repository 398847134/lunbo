<template>
  <div class="main">
      <h1>测试上传</h1>
      <transition-group name='image' tag="ul" class="swiper_img_ul">
        <li v-for="(pic,index) in pics" :key="pic" v-show="index==showpic" @touchstart="stop" @touchend="start">
          <img :src="pic" @touchmove="change" @touchstart="getposition" @touchend="turn">
        </li>
      </transition-group>
  </div>
</template>

<script>
export default {
  name: 'lunBo',
  data () {
    return {
      pics: [], // 轮播图片
      showpic: 0, // 控制图片是否显示
      positionX: 0, // 鼠标点击的坐标
      moveposition: 0 // 移动的当前坐标
    }
  },
  methods: {
    goto (i) {
      this.showpic = i
      this.stop()
      this.start()
    },
    stop () {
      clearInterval(this.timer)
    },
    start () {
      clearInterval(this.timer)
      this.timer = setInterval(this.changeIndex, 5000)
    },
    getposition (e) {
      this.positionX = e.changedTouches[0].clientX
    },
    turn (e) {
      let ta = this.moveposition - this.positionX
      if (ta > 0) {
        if (ta > window.screen.width / 2) {
          this.goto(this.showpic - 1)
        }
      } else {
        if (-ta > window.screen.width / 2) {
          this.goto(this.showpic + 1)
        }
      }
    },
    change (e) {
      this.moveposition = e.changedTouches[0].clientX
    },
    changeIndex () {
      if (this.showpic >= this.pics.length - 1) {
        this.showpic = 0
      } else {
        this.showpic++
      }
    }

  }
}
</script>

<style scoped>
body{
  margin: 0;
  padding: 0;
  background-color: #f2f2f2;
}
img{
  width: 100%;
}
ul{
  list-style: none;
  overflow: hidden;
  height: 500px;
  margin: 0;
  padding:0;
}
.image-enter-active {
  transform: translateX(0);
  transition: all 1s ease;
}

.image-leave-active {
  transform: translateX(-100%);
  transition: all 1s ease;
}

.image-enter {
  transform: translateX(100%)
}

.image-leave {
  transform: translateX(0)
}
li{
  width: 100%;
  float: left;
}
.swiper_img_ul{
  height:562px;
}
.swiper_img_ul img{
  position:absolute;
}
#swiper_img_box{
  position:relative;
  width:100%;
  height:562px;
  overflow:hidden;
}
</style>
