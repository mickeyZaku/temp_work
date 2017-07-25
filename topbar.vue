<template>
  <!--顶导-->
  <div id="Topbar" class="topbar">
    <Row type="flex">
      <Col :span="12">
        <router-link :to="{path: '/order/index'}" class="yiche_logo"></router-link>
        <i class="logo_line">|</i>
        <span class="bar_left">{{ name }}</span>
      </Col>
      <Col :span="12" class="bar_right">
        欢迎您 {{ username }}
        <i class="cut_line">|</i>
      <span @click="loginOut">
        <Icon type="power" ></Icon>
      </span>
      </Col>
    </Row>
  </div>
</template>
<script>
  export default {
    name: 'topbar',
    data () {
      return {
        name: '智能广告投放平台',
        username: ''
      }
    },
    created () {
      this.getUserName()
    },
    methods: {
      getUserName () {
        let self = this
        this.$http.get('/user/loginUserInfo').then(function (res) {
          if (res.data.errorCode === 0) {
            self.username = res.data.result.userName
          }
        })
      },
      loginOut () {
        console.log(444455);
        let xmlhttp = new XMLHttpRequest();

        let url = 'http://dsp.yiche.com/common-portal/common/portal/logout';
        xmlhttp.open('GET', url, true);
//          xmlHttp.onreadystatechange=favorOK;//发送事件后，收到信息了调用函数
        xmlhttp.send();
        console.log(444555);
        console.log(xmlhttp);
        if (xmlhttp.readyState === 4 && xmlhttp.status === 200) {
          let d = xmlhttp.responseText;
          console.log(33);
          console.log(d);
          console.log(233);
        }
        window.location.reload();
//        this.$http.post('/common-portal/common/portal/logout').then(function (res) {
//          console.log(res)
//        })
      }
    }
  }
</script>
