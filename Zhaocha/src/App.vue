<template>
  <div id="app">

    <!--开始前-->
    <div v-if="!startState">
      <img class="image" src="../src/assets/zhaocha/head.jpg"/>
      <div style="margin-top: 10px;font-size: 20px;font-weight: bold">游戏规则</div>
      <div class="rule">
        <div>1、页面每次会出现两张图，上面的是正确广告宣传图，下面的是错误广告宣传图。</div>
        <div>2、每组图的找茬时间为1分钟，每组图都需找到3个找茬点才能进入下一关卡，共设置了3个关卡，完成3个找茬关卡才算完成任务。</div>
        <div>3、每位客户有两次游戏机会，完成3个找茬关卡后可获得10元话费。</div>
      </div>
      <div class="start" v-on:click="startGame(beforeStartTime)" v-show="time != 0">开始游戏</div>

      <!--开始倒计时-->
      <transition-group name="fade">
        <div class="vertical-center" v-if="isModal" :key="1">{{ beforeStartTime }}</div>
        <div class="modal" v-if="isModal" :key="1"></div>
      </transition-group>
    </div>

    <div v-if="startState">
      <!--<h3 v-if="time != endTime" v-bind:class="{ textOrange: time <= 30,textRed: time <= 10 }" class="textGreen">剩余时间:{{ time }}</h3>-->
      <div class="head-bg">
        <div v-if="time != endTime">
          <div class="lastTime">
            时间:{{ time }}
          </div>

          <div class="progress">
            找到:{{ currentCount }}/3
          </div>
        </div>
        <h3 class="reStart" v-if="time == endTime" v-on:click="reStart">重新开始</h3>
      </div>

      <transition-group name="fade">
        <div id="game1" class="game" v-if="progressStep == 0" :key="1">
          <!--<div class="titlePadding">正确图</div>-->
          <img class="image" src="../src/assets/zhaocha/kr.jpg"/>
          <div class="titlePadding"></div>
          <img class="image" src="../src/assets/zhaocha/kw.jpg"/>
          <div id="c1" class="cycle" v-on:click="select(c1,$event)">
            <img v-if="c1" src="../src/assets/zhaocha/correct.png"/>
          </div>
          <div id="c2" class="cycle" v-on:click="select(c2,$event)">
            <img v-if="c2" src="../src/assets/zhaocha/correct.png"/>
          </div>
          <div id="c3" class="cycle" v-on:click="select(c3,$event)">
            <img v-if="c3" src="../src/assets/zhaocha/correct.png"/>
          </div>
        </div>
      </transition-group>

      <transition-group name="fade">
        <div id="game2" class="game" v-if="progressStep == 1" :key="1">
          <!--<div class="titlePadding">正确图</div>-->
          <img class="image" src="../src/assets/zhaocha/cr.jpg"/>
          <div class="titlePadding"></div>
          <img class="image" src="../src/assets/zhaocha/cw.jpg"/>
          <div id="c4" class="cycle" v-on:click="select(c1,$event)">
            <img v-if="c1" src="../src/assets/zhaocha/correct.png"/>
          </div>
          <div id="c5" class="cycle" v-on:click="select(c2,$event)">
            <img v-if="c2" src="../src/assets/zhaocha/correct.png"/>
          </div>
          <div id="c6" class="cycle" v-on:click="select(c3,$event)">
            <img v-if="c3" src="../src/assets/zhaocha/correct.png"/>
          </div>
        </div>
      </transition-group>

      <transition-group name="fade">
        <div id="game3" class="game" v-if="progressStep == 2" :key="1">
          <!--<div class="titlePadding">正确图</div>-->
          <img class="image" src="../src/assets/zhaocha/lr.jpg"/>
          <div class="titlePadding"></div>
          <img class="image" src="../src/assets/zhaocha/lw.jpg"/>
          <div id="c7" class="cycle" v-on:click="select(c1,$event)">
            <img v-if="c1" src="../src/assets/zhaocha/correct.png"/>
          </div>
          <div id="c8" class="cycle" v-on:click="select(c2,$event)">
            <img v-if="c2" src="../src/assets/zhaocha/correct.png"/>
          </div>
          <div id="c9" class="cycle" v-on:click="select(c3,$event)">
            <img v-if="c3" src="../src/assets/zhaocha/correct.png"/>
          </div>
        </div>
      </transition-group>
    </div>
  </div>
</template>

<script>

export default {
  name: 'app',
  data () {
    return {
        time: 10,//每局游戏时间
        endTime: 0,//结束时间
        startState: 0,//是否开启游戏
        isModal: false,//遮罩
        beforeStartTime: 3,//开始前提示时间
        progressStep: 0,//第几个游戏
        currentCount:0,//当前游戏选中几次
        correctCount: 0,//总的选中次数
        c1: false,
        c2: false,
        c3: false,
        showStyle: false,
        openid:''
    }
  },
  methods:{
      //开始游戏
      startGame: function (count) {

          let self = this;
          self.beforeStartTime = count;
          if (count === 3) self.isModal = true;

          if (count === 0) {
              self.isModal = false;
              self.startState = true;
              this.game(self.time);
          } else {
              count -= 1;
              setTimeout(function () {
                  self.startGame(count);
              }, 1000);
          }
      },
       //重新开始游戏
      reStart: function () {
          document.location.reload();
      },
      //倒计时
      game: function (lastTime) {
          let self = this;
          //游戏结束
          if (self.correctCount === 9) return
          //每局游戏重置时间
          if (self.progressStep === 1 || self.progressStep === 2) self.time = 60;
          self.time = lastTime;

          if (self.time === self.endTime) {
              //游戏失败，时间到
              self.submit('-1');
          } else {
              self.time -= 1;
              setTimeout(function () {
                  self.game(self.time);
              }, 1000);
          }
      },
      //选对
      select: function (state,event) {

          let self = this;
          if (state || self.time === self.endTime || self.correctCount === 9) return;

          let arr1 = ['c1','c4','c7'];
          let arr2 = ['c2','c5','c8'];
          let arr3 = ['c3','c6','c9'];
          if (self.isContainObj(arr1,event.target.id)) {
              self.c1 = true;
          }else if (self.isContainObj(arr2,event.target.id)) {
              self.c2 = true;
          }else if (self.isContainObj(arr3,event.target.id)) {
              self.c3 = true;
          }
          self.correctCount++;
          self.currentCount++;

          //下一个游戏
          if (self.correctCount === 3 || self.correctCount === 6) {

              setTimeout(function () {
                  if (self.correctCount === 3) alert('恭喜你！你已完成关卡1的任务！请继续到关卡2');
                  if (self.correctCount === 6) alert('恭喜你！你已完成关卡2的任务！请继续到关卡3');
                  self.c1 = false;
                  self.c2 = false;
                  self.c3 = false;
                  self.time = 60;
                  self.currentCount = 0;
                  self.progressStep = self.correctCount / 3;
              }, 1000);
          }else if (self.correctCount === 9 ) {
              //游戏成功
              self.submit('0');
          }
      },
      //判断数组包含某个元素
      isContainObj: function (arr,value) {
          for (let i = 0; i < arr.length; i++) {
              if (value === arr[i]) {
                  return true;
              }
          }
          return false;
      },
      toast: function (msg, duration) {
          duration = isNaN(duration) ? 3500 : duration;
          let m = document.createElement('div');
          m.innerHTML = msg;
          m.style.cssText = "padding: 5px 10px 5px 10px;font-size:13px; width:70%; min-width:150px; background:#000; color:#fff; text-align:center; border-radius:5px; position:fixed; top:40%; left:15%; z-index:999999;";
          document.body.appendChild(m);
          setTimeout(function () {
              var d = 0.5;
              m.style.webkitTransition = '-webkit-transform ' + d + 's ease-in, opacity ' + d + 's ease-in';
              m.style.opacity = '0';
              setTimeout(function () {
                  document.body.removeChild(m)
              }, d * 1000);
          }, duration);
      },
      //查询url参数
      getQueryString: function (name) {
          var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)");
          var r = window.location.search.substr(1).match(reg);
          if (r != null) return unescape(r[2]);
          return null;
      },
      //提交数据
      submit: function (status) {

          let self = this;
          let userid = self.getQueryString('openid');
          let param = '?openid='+userid+'&gamests='+status;

          self.$ajax({
              method: 'get',
              url: 'http://weixin.gxhzyd.cn/games/zhaochaget.asp'+param,
              headers: {
                  'Content-Type': 'application/x-www-form-urlencoded',
              },
          }).then(function (response) {
              //请求数据
              if (response.status === 200) {
                  let data = response.data;
                  console.log(data);
                  if (data.errcode === '-1') {
                      self.toast(data.errmsg);
                  }else if (data.errcode === '0'){
                      self.toast('恭喜你成功获得20元话费!');
                  }
              }
          }).catch(function (error) {
              console.log(error);
          });
      },

  },
    created: function (){
      console.log('start')

    }

}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.image {
  width: 100%;
}

.rule {
  text-align: left;
  font-size: 17px;
  margin-top: 10px;
  padding-left: 10px;
  padding-right: 10px;
}

.start {
  width: 200px;
  height: 40px;
  border-radius: 20px;
  background-color: #62b900;
  color: white;
  line-height: 40px;
  margin: 0 auto;
  margin-top: 20px;
}

.reStart {

  width: 200px;
  height: 40px;
  border-radius: 20px;
  background-color: #62b900;
  color: white;
  line-height: 40px;
  position: absolute;
  left: 50%;
  top: 10px;
  margin-left: -100px;
}

.modal {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  overflow: hidden;
  outline: 0;
  -webkit-overflow-scrolling: touch;
  background-color: rgb(0, 0, 0);
  filter: alpha(opacity=60);
  background-color: rgba(0, 0, 0, 0.6);
  z-index: 9998;
}

.vertical-center {
  width: 200px;
  height: 200px;
  position: fixed;
  left: 50%;
  top: 50%;
  z-index: 9999;
  margin: -100px 0 0 -100px;
  color: white;
  font-size: 80px;
  font-weight: bold;
}

.head-bg {
  background-image: url("assets/zhaocha/bg.png");
  width: 100%;
  height: 100px;
  background-size:cover;
  margin-bottom: 5px;
}

.lastTime {
  background-image: url("assets/zhaocha/mp.png");
  width: 120px;
  height: 80px;
  background-size: cover;
  display: inline-block;
  line-height: 80px;
  font-size: 17px;
  color: white;
  padding-top: 15px;
  font-weight: bold;
  background-position:center;
}

.progress {
  background-image: url("assets/zhaocha/mp.png");
  width: 120px;
  height: 80px;
  background-size: cover;
  display: inline-block;
  line-height: 80px;
  font-size: 17px;
  color: white;
  padding-top: 15px;
  font-weight: bold;
  background-position:center;
}

.titlePadding {
  padding: 10px 0 10px 0;
}

.cycle {
  width: 50px;
  height: 50px;
  text-align: right;
}

.game{
  position: relative;
}

.textGreen {
  color: #62b900;
}

.textOrange {
  color: orange;
}

.textRed {
  color: red;
}

#c1 {
  position: absolute;
  top: 8%;
  left: 35%;
  height: 60px;
  width: 80px;
  text-align: right;
}

#c2 {
  position: absolute;
  top: 20%;
  right: 25%;
}

#c2 img {
  position: absolute;
  bottom: -25px;
  right: -20px;
}

#c3 {
  position: absolute;
  top: 20%;
  right: 5%;
}

#c3 img {
  position: absolute;
  bottom: -25px;
  right: -20px;
}

#c4 {
  position: absolute;
  top: 22%;
  right: 38%;
  width: 80px;
  /*background-color: yellow;*/
}

#c4 img {
  position: absolute;
  bottom: -20px;
  right: -20px;
}


#c5 {
  position: absolute;
  top: 32%;
  left: 30%;
}

#c6 {
  position: absolute;
  top: 32%;
  right: 30%;
  /*background-color: yellow;*/
}

#c7 {
  position: absolute;
  top: 12%;
  left: 10%;
  width: 120px;
}

#c8 {
  position: absolute;
  top: 20%;
  left: 10%;
  width: 120px;
  height: 40px;
}

#c9 {
  position: absolute;
  top: 30%;
  left: 10%;
  width: 150px;
  height: 40px;
  text-align: center;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity .5s
}
.fade-enter, .fade-leave-to /* .fade-leave-active in below version 2.1.8 */ {
  opacity: 0
}

.textToast{z-index:2147483647; width:60%; height:32px; line-height:32px; font-size:13px; text-align:center; color:#fff !important; border-radius:5px; background-color:rgba(80,80,80,1); position:fixed; left:50%; top:50%; -webkit-transform:translate(-50%,-50%); transform:translate(-50%,-50%); display:none;}
.textToastShow{z-index:2147483647; display:block; -webkit-animation:textToastShow 3.0s .2s ease-in-out; animation:textToastShow 3.0s .2s ease-in-out;}
</style>
