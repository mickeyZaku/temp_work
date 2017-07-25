<template>
<div>
  <!--侧边栏-->
  <Menu :active-name="menus.selected" theme="dark" width="auto">
    <Menu-item v-for="(item, index) in menus.items" :key="index" :name="index">
      <router-link class="aside-style" :to="{path: item.path}" exact>
        <Icon :type="item.icon"></Icon>
        <p class="layout-text">{{item.name}}</p>
      </router-link>
    </Menu-item>
  </Menu>
</div>
</template>
<script>
  export default {
    name: 'navigation',
    created () {
      this.judgePath()
    },
    data () {
      return {
        menus: {
          items: [
            {
              name: '订单管理',
              icon: 'filing',
              path: '/order/index',
              actived: true
            },
            {
              name: '投放管理',
              icon: 'ios-keypad',
              path: '/serving/plan/indexplan',
              actived: false
            },
            {
              name: '数据报告',
              icon: 'document-text',
              path: '',
              actived: false
            }
          ],
          selected: 0
        }
      }
    },
    methods: {
      judgePath () {
        let url = window.location.href
        if (url.indexOf('/serving') !== -1) {
          this.menus.selected = 1
        }
      }
    }
  }
</script>
<style>
  .aside-style {
    padding: 14px 24px;
    width: 100%;
    height: 100%;
    display: inline-block;
  }
  .ivu-menu-vertical .ivu-menu-item {
    padding: 0;
  }
</style>
