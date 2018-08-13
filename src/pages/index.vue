<template>
  <div class="container" @click="clickHandle('test click', $event)">
    {{welcomeMessage}}

    <div class="author-check" v-if="!isAuthorized">
      <button open-type="getUserInfo">安保权限检测</button>
    </div>

    <div class="userinfo" v-if="isAuthorized" @click="bindViewTap">
      <img class="userinfo-avatar" v-if="userInfo.avatarUrl" :src="userInfo.avatarUrl" background-size="cover"/>
      <div class="userinfo-nickname">
        <card :text="userInfo.nickName"></card>
      </div>
    </div>

    <!--<div class="usermotto">-->
    <!--<div class="user-motto">-->
    <!--<card :text="motto"></card>-->
    <!--</div>-->
    <!--</div>-->

    <!--<form class="form-container">-->
    <!--<input type="text" class="form-control" v-model="motto" placeholder="v-model" />-->
    <!--<input type="text" class="form-control" v-model.lazy="motto" placeholder="v-model.lazy" />-->
    <!--</form>-->
    <!--<a href="/pages/counter" class="counter">去往Vuex示例页面</a>-->
  </div>
</template>

<script>
  import card from '@/components/card';

  export default {
    data () {
      return {
        // motto: 'Hello World',
        userInfo: {}
      };
    },

    computed: {
      isAuthorized () {
        return Object.keys(this.userInfo).length > 0;
      },
      welcomeMessage () {
        return this.isAuthorized ? `欢迎回来，已获授权研究员` : `欢迎访问，请验证身份`;
      }
    },

    components: {
      card
    },

    methods: {
      bindViewTap () {
        const url = '/packageA/logs';
        wx.navigateTo({ url });
      },
      getUserInfo () {
        // 调用登录接口
        wx.login({
          success: () => {
            wx.getUserInfo({
              success: (res) => {
                this.userInfo = res.userInfo;
              },
              fail: (res) => {
                if (res.errMsg && res.errMsg.startsWith('getUserInfo:fail')) {
                  console.log(res);
                }
              }
            });
          }
        });
      },
      clickHandle (msg, ev) {
        // eslint-disable-next-line
        console.log("clickHandle:", msg, ev);
      }
    },

    created () {
      // 调用应用实例的方法获取全局数据
      this.getUserInfo();
    }
  };
</script>

<style scoped>
  .userinfo {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .userinfo-avatar {
    width: 100px;
    height: 100px;
    margin-top: 15px;
    border-radius: 50%;
  }

  .userinfo-nickname {
    color: #aaa;
  }

  .usermotto {
    margin-top: 150px;
  }

  .form-control {
    display: block;
    padding: 0 12px;
    margin-bottom: 5px;
    border: 1px solid #ccc;
  }

  .counter {
    display: inline-block;
    margin: 10px auto;
    padding: 5px 10px;
    color: blue;
    border: 1px solid blue;
  }
</style>
