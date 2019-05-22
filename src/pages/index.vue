<template>
  <div id="app" style="margin: 30px;">


    <view style="min-height: 300px">

      <div>

        <p style="font-size: 16px">学号：</p>
        <input placeholder="请输入学号" v-model="input_id" style="margin: 5px"/>
        <p style="font-size: 16px">姓名:</p>
        <input placeholder="请输入姓名" v-model="input_name" style="margin: 5px;"/>

      </div>


    </view>


    <view>
      <button type="primary" open-type="getUserInfo" lang="zh_CN" style="margin-top: 20px;"
              @getuserinfo="onGotUserInfo">签到
      </button>
    </view>


  </div>

</template>

<script>


  export default {
    data() {
      return {
        input_id: null,
        input_name: null,

      }
    },

    components: {
    },

    methods: {
      onGotUserInfo(e) {
        console.log(e.mp);
        console.log(e.mp.detail.signature);
        if (this.input_id === null || this.input_name === null) {
          wx.showToast({
            title: '请填写完整',
            icon: 'none',
            duration: 2000
          })
        } else {
          this.showWarn()
        }
      },
      showWarn() {
        wx.showModal({
          title: '提示',
          content: '每个微信号只能签到一次，请仔细检查个人信息是否正确',
          success: res => {
            if (res.confirm) {
              console.log('用户点击确定');
              this.scanCodeSignIn()
            } else if (res.cancel) {
              console.log('用户点击取消')
            }
          }
        })
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
