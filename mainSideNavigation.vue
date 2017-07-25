<template>
<div class="main-side">
  <!--侧边栏-->
  <Menu :active-name="menus.selected" :open-names="menus.open" @on-select="changeMenuColor" theme="dark" width="auto">
    <Menu-item name="0">
      <router-link class="single-title order-left-control" :to="{path: '/order/index'}" exact>
        <span class="side-blue"></span>
      <Icon class="iconfont icon-toufangguanli-navi icon-order-gray-normal sideBarIcon " style="margin-left: 26px"></Icon>
      <span style="margin-left: -10px;">订单管理</span>
      </router-link>
    </Menu-item>
    <Submenu name="1">
      <template slot="title">
        <span class="side-blue"></span>
        <Icon class="iconfont icon-baogao-navi icon-adserving-default sideBarIcon"></Icon>
        <span>投放管理</span>
      </template>
      <Menu-item name="1-1">
        <router-link :to="{path: '/serving/plan/indexplan'}" exact>
          <div class="only-two">
            <i><span></span></i>
            广告计划
          </div>
        </router-link>
      </Menu-item>
      <Menu-item name="1-2">
        <router-link :to="{path: '/serving/group/indexgroup'}" exact>
          <div class="only-two">
            <i><span></span></i>
            广告组
          </div>
        </router-link>
      </Menu-item>
      <Menu-item name="1-3">
        <router-link :to="{path: '/serving/materials/indexmaterials'}" exact>
          <div class="only-two">
            <i><span></span></i>
            物料
          </div>
        </router-link>
      </Menu-item>
    </Submenu>
    <Submenu name="2">
      <template slot="title">
        <span class="side-blue"></span>
        <Icon class="iconfont icon-wenjian-navi icon-data-report-default sideBarIcon"></Icon>
        数据报告
      </template>
      <Menu-group title="账户报表" :class="{mark1:markFirstMenu}">
        <Menu-item name="2-1-1">
          <router-link :to="{path: '/data/accountList/client'}"  exact>
            <i><span></span></i>
            客户
          </router-link>
        </Menu-item>
      </Menu-group>
      <Menu-group title="订单报表" :class="{mark2:markSecondMenu}">
        <Menu-item name="2-2-1">
          <router-link :to="{path: '/data/orderList/order'}" exact>
            <i><span></span></i>
            订单
          </router-link>
        </Menu-item>
        <Menu-item name="2-2-2">
          <router-link :to="{path: '/data/orderList/ad'}" exact>
            <i><span></span></i>
            广告位
          </router-link>
        </Menu-item>
        <Menu-item name="2-2-3">
          <router-link :to="{path: '/data/orderList/materials'}" exact>
            <i><span></span></i>
            物料
          </router-link>
        </Menu-item>
      </Menu-group>
    </Submenu>
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
          selected: '0',
          open: ['0', '1', '2']
        },
        markSecondMenu: false,
        markFirstMenu: false
      }
    },
    methods: {
      judgePath () {
        let url = window.location.href
        if (url.indexOf('/serving/plan') !== -1) {
          this.menus.selected = '1-1'
          this.menus.open = ['1']
        } else if (url.indexOf('/serving/group') !== -1) {
          this.menus.selected = '1-2'
          this.menus.open = ['1']
          console.log(this.menus.open)
        } else if (url.indexOf('/serving/materials') !== -1) {
          this.menus.selected = '1-3'
          this.menus.open = ['1']
        } else if (url.indexOf('data/accountList/client') !== -1) {
          this.menus.selected = '2-1-1'
          this.menus.open = ['2']
        } else if (url.indexOf('data/orderList/order') !== -1) {
          this.menus.selected = '2-2-1'
          this.menus.open = ['2']
        } else if (url.indexOf('data/orderList/ad') !== -1) {
          this.menus.selected = '2-2-2'
          this.menus.open = ['2']
        } else if (url.indexOf('data/orderList/materials') !== -1) {
          this.menus.selected = '2-2-3'
          this.menus.open = ['2']
        }
      },
      changeMenuColor (name) {
        if (/2-1-\d/.test(name)) {
          this.markFirstMenu = true;
          this.markSecondMenu = false;
        } else if (/2-2-\d/.test(name)) {
          this.markSecondMenu = true;
          this.markFirstMenu = false;
        } else {
          this.markFirstMenu = false;
          this.markSecondMenu = false;
        }
      }
    }
  }
</script>
<style>
  .chooseO {
    display: inline-block;
    border-radius: 50%;
    border: 1px solid #BBC0CA;
    width: 6px;
    height: 6px;
  }
  .blueO{
    background: #3d70fb;
  }
  .layout-text {
    color: #BBC0CA;
  }
  .layout-text:hover{
    color: #fff;
  }
  .aside-style {
    padding: 14px 24px;
    width: 100%;
    height: 100%;
    display: inline-block;
  }
  .ivu-menu-vertical .ivu-menu-item {
    padding: 0;
  }
  .ivu-menu-dark.ivu-menu-vertical .ivu-menu-item-group-title{
    height:56px;
    color: #bbc0ca;
  }
  .ivu-menu-vertical .ivu-menu-item-group-title{
    padding-left: 60px;
  }
  .sidebar .ivu-menu-dark.ivu-menu-vertical .ivu-menu-item .order-left-control{
    margin-left: -36px;
  }
  .layout .sidebar .ivu-menu-dark.ivu-menu-vertical .ivu-menu-item:hover a i span{
    background: #3d70fb;
    border:1px solid #3d70fb;
  }
  .layout .sidebar .ivu-menu-dark.ivu-menu-vertical .ivu-menu-item:hover a{
    color:white;
  }
  .side-blue{
    position: absolute;
    background:#3d70fb;
    width:4px;
    height:100%;
    left:0;
    top:0;
  }
  .ivu-menu-dark.ivu-menu-vertical .ivu-menu-opened {
    background: #252834;

  }.ivu-menu-dark.ivu-menu-vertical .ivu-menu-opened .ivu-menu-submenu-title{
    background: #252834;
  }

  .ivu-menu-dark.ivu-menu-vertical .ivu-menu-item-active .ivu-menu-submenu-title{
    background: #252834;
  }

  .ivu-menu-dark .ivu-menu-item .side-blue, .ivu-menu-vertical .ivu-menu-opened .ivu-menu-submenu-title .side-blue{
    display: none;
  }
  .ivu-menu-dark .ivu-menu-item:hover .side-blue, .ivu-menu-vertical .ivu-menu-opened .ivu-menu-submenu-title:hover .side-blue{
    display: block;
  }
  .ivu-menu-dark.ivu-menu-vertical .ivu-menu-item-active .side-blue{
    display: block;
  }
  .ivu-menu-submenu .side-blue{
    display: none;
  }
  .ivu-menu-submenu:hover .side-blue{
    display: block;
  }
  .ivu-menu-dark.ivu-menu-vertical .ivu-menu-item-active:not(.ivu-menu-submenu), .ivu-menu-dark.ivu-menu-vertical .ivu-menu-item-active:not(.ivu-menu-submenu):hover, .ivu-menu-dark.ivu-menu-vertical .ivu-menu-submenu-title-active:not(.ivu-menu-submenu), .ivu-menu-dark.ivu-menu-vertical .ivu-menu-submenu-title-active:not(.ivu-menu-submenu):hover{
    background: #20242e!important;
  }
  .only-two{
    margin-left:-28px;
  }
  .mark1 .ivu-menu-item-group-title {
    color:#fff !important;
  }
  .mark2 .ivu-menu-item-group-title {
    color:#fff !important;
  }
</style>
