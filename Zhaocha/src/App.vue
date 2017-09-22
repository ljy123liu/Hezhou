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
    </div>

    <!--倒计时-->
    <transition-group name="fade">
      <div class="vertical-center" v-if="isModal" :key="1">{{ beforeStartTime }}</div>
      <div class="modal" v-if="isModal" :key="1"></div>
    </transition-group>

    <div v-if="startState">
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

      <!--过关提示-->
      <div v-show="alert" :key="1">
        <div class="vertical-center alert">
          <img class="gou" src="../src/assets/zhaocha/gou.png"/>
          <div class="tips">{{ tips }}</div>
          <div class="next" v-on:click="next">确定</div>
        </div>
        <div class="modal"></div>
      </div>

      <transition-group name="fade">
        <div id="game1" class="game" v-if="progressStep == 0" :key="1">
          <div style="position: relative">
            <img src="../src/assets/zhaocha/kr.jpg" style="width: 100%"/>
            <div class="c1" v-on:click="select(c1,$event)">
              <img v-if="c1" src="../src/assets/zhaocha/correct.png"/>
            </div>
            <div class="c2" v-on:click="select(c2,$event)">
              <img v-if="c2" src="../src/assets/zhaocha/correct.png"/>
            </div>
            <div class="c3" v-on:click="select(c3,$event)">
              <img v-if="c3" src="../src/assets/zhaocha/correct.png"/>
            </div>
          </div>

          <div class="item-top">
            <img src="../src/assets/zhaocha/kw.jpg" style="width: 100%"/>
            <div class="c1" v-on:click="select(c1,$event)">
              <img v-if="c1" src="../src/assets/zhaocha/correct.png"/>
            </div>
            <div class="c2" v-on:click="select(c2,$event)">
              <img v-if="c2" src="../src/assets/zhaocha/correct.png"/>
            </div>
            <div class="c3" v-on:click="select(c3,$event)">
              <img v-if="c3" src="../src/assets/zhaocha/correct.png"/>
            </div>
          </div>
        </div>

      </transition-group>

      <transition-group name="fade">
        <div id="game3" class="game" v-if="progressStep == 1" :key="1">

          <div style="position: relative">
            <img src="../src/assets/zhaocha/lr.jpg" style="width: 100%"/>
            <div class="c4" v-on:click="select(c1,$event)">
              <img v-if="c1" src="../src/assets/zhaocha/correct.png"/>
            </div>
            <div class="c5" v-on:click="select(c2,$event)">
              <img v-if="c2" src="../src/assets/zhaocha/correct.png"/>
            </div>
            <div class="c6" v-on:click="select(c3,$event)">
              <img v-if="c3" src="../src/assets/zhaocha/correct.png"/>
            </div>
          </div>

          <div class="item-top">
            <img src="../src/assets/zhaocha/lw.jpg" style="width: 100%"/>
            <div class="c4" v-on:click="select(c1,$event)">
              <img v-if="c1" src="../src/assets/zhaocha/correct.png"/>
            </div>
            <div class="c5" v-on:click="select(c2,$event)">
              <img v-if="c2" src="../src/assets/zhaocha/correct.png"/>
            </div>
            <div class="c6" v-on:click="select(c3,$event)">
              <img v-if="c3" src="../src/assets/zhaocha/correct.png"/>
            </div>
          </div>
        </div>

      </transition-group>

      <transition-group name="fade">
        <div id="game2" class="game" v-if="progressStep == 2" :key="1">
          <div style="position: relative">
            <img src="../src/assets/zhaocha/cr.jpg" style="width: 100%"/>
            <div class="c7" v-on:click="select(c1,$event)">
              <img v-if="c1" src="../src/assets/zhaocha/correct.png"/>
            </div>
            <div class="c8" v-on:click="select(c2,$event)">
              <img v-if="c2" src="../src/assets/zhaocha/correct.png"/>
            </div>
            <div class="c9" v-on:click="select(c3,$event)">
              <img v-if="c3" src="../src/assets/zhaocha/correct.png"/>
            </div>
          </div>

          <div class="item-top">
            <img src="../src/assets/zhaocha/cw.jpg" style="width: 100%"/>
            <div class="c7" v-on:click="select(c1,$event)">
              <img v-if="c1" src="../src/assets/zhaocha/correct.png"/>
            </div>
            <div class="c8" v-on:click="select(c2,$event)">
              <img v-if="c2" src="../src/assets/zhaocha/correct.png"/>
            </div>
            <div class="c9" v-on:click="select(c3,$event)">
              <img v-if="c3" src="../src/assets/zhaocha/correct.png"/>
            </div>
          </div>
        </div>
      </transition-group>


      <div v-if="end" :key="1">
        <div class="modal">
          <div class="endStyle">
            <img v-on:click="closeWindow" class="close" src="../src/assets/zhaocha/close.png"/>
            <img src="../src/assets/zhaocha/cr.jpg" style="width: 100%"/>
            <div style="font-size: 21px;text-align: center;padding: 10px">恭喜你成功获得10元话费!</div>
            <div style="font-size: 17px;text-align: center;padding-left: 10px;padding-right: 10px">到广西移动APP了解存话费送话费</div>
            <div class="next" v-on:click="checkActivity">确定</div>
          </div>
        </div>
      </div>

    </div>
  </div>
</template>

<script>

    export default {
        name: 'app',
        data () {
            return {
                time: 60,//每局游戏时间
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
                alert: false,
                openid:'',
                tips:'',//过关提示文字
                end: 0
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
                if (self.correctCount === 9 || self.currentCount === 3) return
                //每局游戏重置时间
                if (self.progressStep === 1 || self.progressStep === 2) self.time = 60;
                self.time = lastTime;

                if (self.time === self.endTime) {
                    //游戏失败，时间到
                    self.submit('0');
                } else {
                    self.time -= 1;
                    setTimeout(function () {
                        self.game(self.time);
                    }, 1000);
                }
            },
            //选对
            select: function (state,event) {
//          console.log(event);
                let self = this;
                if (state || self.time === self.endTime || self.correctCount === 9) return;

                let arr1 = ['c1','c4','c7'];
                let arr2 = ['c2','c5','c8'];
                let arr3 = ['c3','c6','c9'];
                if (self.isContainObj(arr1,event.target.className)) {
                    self.c1 = true;
                }else if (self.isContainObj(arr2,event.target.className)) {
                    self.c2 = true;
                }else if (self.isContainObj(arr3,event.target.className)) {
                    self.c3 = true;
                }
                self.correctCount++;
                self.currentCount++;

                if (self.correctCount === 3) {
                    self.tips = '恭喜你！你已完成关卡1的任务！请继续到关卡2';
                    self.alert = true;
                }
                if (self.correctCount === 6) {
                    self.tips = '恭喜你！你已完成关卡2的任务！请继续到关卡3';
                    self.alert = true;
                }
                if (self.correctCount === 9 ) {
                    //游戏成功
                    self.submit('1');
                }
            },
            next: function () {
                let self = this;
                self.alert = false;
                self.c1 = false;
                self.c2 = false;
                self.c3 = false;
                self.time = 60;
                self.currentCount = 0;
                self.progressStep = self.correctCount / 3;
                self.game(self.time);
            },
            checkActivity: function () {
                window.location.href = 'http://gx.10086.cn/zt-portal/wap/promoaccept/tchy_dp12y_jsb/index';
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
                m.style.cssText = "padding: 10px 10px 10px 10px;font-size:17px; width:70%; left:15%; background-color: rgba(0, 0, 0, 0.6); color:#fff; text-align:center; border-radius:5px; position:fixed; top:40%;  z-index:999999;";
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
                        if (data.errcode === '-1') {
                            self.toast(data.errmsg);
                        }else if (data.errcode === '0'){
                            self.end = true;
                        }else if (data.errcode === '1001') {
                            self.toast(data.errmsg);
                        }
                    }
                }).catch(function (error) {
                    console.log(error);
                });
            },
            isWeixn: function () {
                var ua = navigator.userAgent.toLowerCase();
                if (ua.match(/MicroMessenger/i) == "micromessenger") {
                    return true;
                } else {
                    return false;
                }
            },
            closeWindow: function () {
                let self = this;
                if (self.isWeixn()) {
                    WeixinJSBridge.call('closeWindow');
                }else {
                    window.opener=null;
                    window.open('','_self');
                    window.close();
                }
            }
        },
        created: function (){
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
    padding-left: 15px;
    padding-right: 15px;
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
    margin-bottom: 20px;
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
    /*left: 50%;*/
    /*margin-left: -207px;*/
    overflow: hidden;
    outline: 0;
    -webkit-overflow-scrolling: touch;
    background-color: rgb(0, 0, 0);
    filter: alpha(opacity=60);
    background-color: rgba(0, 0, 0, 0.6);
    z-index: 9998;
    /*max-width: 414px;*/
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
    /*margin-bottom: 5px;*/
  }

  .item-top {
    position: relative;
    margin-top: 15px;
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

  .game{
    position: relative;
  }

  .textGreen {
    color: #62b900;
  }

  .alert {
    width: 240px;
    height: 240px;
    background-color: white;
    border-radius: 5px;
    color: black;
    font-size: 17px;
    margin-left: -120px;
  }

  .gou {
    margin-top: 15px;
    width: 64px;
    height: 64px;
  }

  .tips {
    position: absolute;
    padding: 15px 15px 15px 15px;
    font-weight: 400;
  }

  .next {
    height: 50px;
    width: 100px;
    position: absolute;
    bottom: 15px;
    line-height: 50px;
    background-color: rgb(121,201,244);
    color: white;
    border-radius: 3px;
    left: 50%;
    margin-left: -50px;
  }

  .endStyle {
    position: relative;
    margin-top: 100px;
    background-color: white;
    left: 50%;
    margin-left: -140px;
    width: 280px;
    height: 350px;
    z-index: 9999;
  }

  .close {
    width: 20px;
    height: 20px;
    position: absolute;
    top: -10px;
    right: -10px;
  }

  .c1 {
    position: absolute;
    top: 8%;
    left: 35%;
    height: 60px;
    width: 80px;
    text-align: right;
    -webkit-tap-highlight-color:rgba(0,0,0,0);
    /*background-color: yellow;*/
  }

  .c1 img {
    position: absolute;
    bottom: -25px;
    right: -5px;
  }

  .c2 {
    position: absolute;
    top: 45%;
    right: 25%;
    /*background-color: yellow;*/
    width: 50px;
    height: 50px;
    -webkit-tap-highlight-color:rgba(0,0,0,0);
  }

  .c2 img {
    position: absolute;
    bottom: -25px;
    right: -20px;
  }

  .c3 {
    position: absolute;
    top: 45%;
    right: 5%;
    /*background-color: yellow;*/
    width: 50px;
    height: 50px;
    -webkit-tap-highlight-color:rgba(0,0,0,0);
  }

  .c3 img {
    position: absolute;
    bottom: -25px;
    right: -20px;
  }

  .c4 {
    position: absolute;
    top: 30%;
    left: 10%;
    width: 120px;
    height: 40px;
    -webkit-tap-highlight-color:rgba(0,0,0,0);
  }

  .c4 img {
    position: absolute;
    bottom: -10px;
    right: -20px;
  }


  .c5 {
    position: absolute;
    top: 48%;
    left: 15%;
    width: 100px;
    height: 40px;
    text-align: right;
    -webkit-tap-highlight-color:rgba(0,0,0,0);
    /*background-color: yellow;*/
  }

  .c5 img {
    position: absolute;
    bottom: 0px;
    right: -30px;
  }

  .c6 {
    position: absolute;
    top: 65%;
    left: 10%;
    width: 120px;
    height: 40px;
    text-align: right;
    -webkit-tap-highlight-color:rgba(0,0,0,0);
  }

  .c7 {
    position: absolute;
    top: 50%;
    left: 40%;
    width: 120px;
    height: 50px;
    /*background-color: yellow;*/
    text-align: right;
    -webkit-tap-highlight-color:rgba(0,0,0,0);
  }

  .c7 img {
    position: absolute;
    bottom: 0px;
    right: 0px;
  }

  .c8 {
    position: absolute;
    top: 70%;
    left: 20%;
    width: 100px;
    height: 40px;
    /*background-color: yellow;*/
    text-align: right;
    -webkit-tap-highlight-color:rgba(0,0,0,0);
  }

  .c9 {
    position: absolute;
    top: 70%;
    left: 55%;
    width: 100px;
    height: 40px;
    /*background-color: yellow;*/
    text-align: center;
    -webkit-tap-highlight-color:rgba(0,0,0,0);
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
