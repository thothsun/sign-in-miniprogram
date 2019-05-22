<template>
  <div id="app" style="margin: 30px;">


    <view style="min-height: 300px">

      <div>
        <p>姓名：张三</p>
        <p>学号：20151234</p>
      </div>

      <!--<div style="width: 50%">-->
      <!--<button size="mini">修改</button>-->
      <!--</div>-->

    </view>


    <view>
      <button type="primary" open-type="getUserInfo" lang="zh_CN"
              @getuserinfo="onGotUserInfo">签到
      </button>
    </view>


  </div>

</template>

<script>


  export default {
    data() {
      return {}
    },


    methods: {
      onGotUserInfo(e) {
        console.log(e.mp.detail.signature)
        this.scanCodeSignIn()
      },
      scanCodeSignIn() {
        // 只允许从相机扫码
        wx.scanCode({
          onlyFromCamera: true,
          success(res) {
            console.log(res)
          }
        })
      },
      onSignInSuccess() {
        wx.showToast({
          title: '签到成功',
          icon: 'success',
          duration: 2000
        })
      },
      onSignInFailed() {
        wx.showToast({
          title: '已签到，不能重复签到',
          icon: 'none',
          duration: 2000
        })
      }
    },


    created() {

    }
  }
</script>

<style scoped>

  .text_subtitle {
    font-size: x-large;
    color: #353535;
  }

  .text-input {
    height: 22px;
    font-size: 18pt;
    color: #353535;
    margin-bottom: 5px;
  }

  .text_convert {
    font-size: x-large;
    color: #353535;
  }

  .divLine {
    background: #b2b2b2;
    width: 100%;
    height: 1px;
  }
</style>
