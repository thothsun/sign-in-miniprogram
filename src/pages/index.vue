<template>
  <div id="app" style="margin: 30px;">


    <view style="min-height: 300px">

      <div>

        <p style="font-size: 16px">学号：</p>
        <input placeholder="请输入学号" v-model="input_id" style="margin: 5px"/>
        <p style="font-size: 16px">姓名:</p>
        <input placeholder="请输入姓名" v-model="input_name" style="margin: 5px;"/>
        <p style="font-size: 16px">openid:</p>
        <input placeholder="openid" v-model="openid" style="margin: 5px" readonly="readonly">
      </div>

    </view>


    <view>
      <button type="primary" @click="signIn" lang="zh_CN" style="margin-top: 20px;"
      >签到
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
        openid: null
      }
    },

    components: {},

    methods: {
      getUserOpenId() {
        wx.login({
          success: res => {
            wx.request({
              url: 'https://api.ai-developer.net/wechat/openid',
              data: {
                code: res.code
              },
              method:'GET',
              header: {
                'content-type': 'application/json'
              },
              success: res => {
                console.log(res.data.data.openid);
                this.openid = res.data.data.openid
              }
            })
          }
        })
      },
      signIn() {
        if (this.openid === null) {
          wx.showToast({
            title: '获取微信openid异常',
            icon: 'none',
            duration: 2000
          })
        } else {
          if (this.input_id == null || this.input_name == null || this.input_id === '' || this.input_name === '') {
            wx.showToast({
              title: '请填写完整',
              icon: 'none',
              duration: 2000
            })
          } else {
            this.saveUserInfo();
          }
        }
      },

      saveUserInfo() {
        wx.clearStorageSync();
        wx.setStorageSync('input_id', this.input_id);
        wx.setStorageSync('input_name', this.input_name);
        console.log('缓存完成');
        this.showWarn()
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
          success: res => {
            console.log(res.result);

            wx.request({
              url: res.result,
              data: {
                stuID: this.input_id,
                stuName: this.input_name
              },
              header: {
                'content-type': 'application/json' // 默认值
              },
              method:'POST',
              success: res => {
                this.onSignInSuccess();
                console.log(res.data)
              },
              fail: res => {
                this.onSignInFailed();
              }
            })
          }
        })
      },
      // TODO   3。修改网络请求回调函数  4。重复、失败、成功
      onSignInSuccess() {
        wx.showToast({
          title: '签到成功',
          icon: 'success',
          duration: 2000
        })
      },
      onSignInFailed() {
        wx.showToast({
          title: '签到失败，网络异常',
          icon: 'none',
          duration: 2000
        })
      },
      onSignInRepeated() {
        wx.showToast({
          title: '已签到，不能重复签到',
          icon: 'none',
          duration: 2000
        })
      }
    },


    created() {
      this.input_id = wx.getStorageSync('input_id');
      this.input_name = wx.getStorageSync('input_name');
      this.getUserOpenId()
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
