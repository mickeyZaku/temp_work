<template>
  <div>
    <Tabs type="card" v-if="tabs.flag" @on-click="changePane" :value="tabs.selected" :animated="false">
      <Tab-pane  :key="index" v-for="(item, index) in tabs.items" :label="item.name" :name="tabs.items.selected">
      </Tab-pane>
    </Tabs>
    <router-view></router-view>
  </div>
</template>
<script>
  export default {
    data () {
      return {
        tabs: {
          flag: true,
          items: [
            {
              name: '广告计划',
              selected: 0
            },
            {
              name: '广告组',
              selected: 1
            },
            {
              name: '物料',
              selected: 2
            }
          ],
          selected: 0
        }
      }
    },
    created () {
      this.active()
    },
    methods: {
      changePane (name) {
        this.tabs.selected = name
        if (name === 0) {
          this.$router.push('/serving/plan/indexplan')
        } else if (name === 1) {
          this.$router.push('/serving/group/indexgroup')
        } else if (name === 2) {
          this.$router.push('/serving/materials/indexmaterials');
        }
      },
      active () {
        let url = window.location.href
        if (url.indexOf('/serving/plan/indexplan') !== -1) {
          this.tabs.flag = true
          this.tabs.selected = 0
        } else if (url.indexOf('/serving/group/indexgroup') !== -1) {
          this.tabs.flag = true
          this.tabs.selected = 1
        } else if (url.indexOf('/serving/materials/indexmaterials') !== -1) {
          this.tabs.flag = true
          this.tabs.selected = 2
        } else {
          this.tabs.flag = false
        }
      }
    }
  }
</script>
