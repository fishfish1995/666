<template>
    <div>
      <div class="fixed-bar">
      <mu-appbar>
        <div class="logo" slot="left"></div>
        <div class="mu-icon-button" >
        	<input type="search" class="mui-input-clear whiteBg" placeholder="大家都在搜索“拯救脱发大油头”" readonly="readonly">
            
        </div>
        <!--<mu-icon-button icon='search'  slot="right"/>-->
      </mu-appbar>
      <mu-tabs :value="activeTab" @change="handleTabChange" class="view-tabs">
        <mu-tab value="rage" title="推荐"/>
        <mu-tab value="songList" title="美妆个护"/>
        <mu-tab value="leaderBoard" title="鞋包配饰"/>
        <mu-tab value="motherandbaby" title="母婴"/>
        <mu-tab value="homeFurnishing" title="家居"/>
        <mu-tab value="more" title="更多"/>
      </mu-tabs>
      </div>
      <div class="default-view" :class="{view: songList.length > 0}">
        <keep-alive>
         <router-view></router-view>
        </keep-alive>
      </div>
    </div>
</template>
<script>
  import { mapGetters } from 'vuex'
  export default {
    data () {
      return {
        activeTab: 'rage'
      }
    },
    created () {
      // 当created函数时监测路由信息,防止页面刷新tab高亮错误
      var tmpArr = this.$route.path.split('/')
      if (tmpArr[1] === 'index') {
        this.handleTabChange(tmpArr[2])
      }
    },
    // watch函数监测路由的变化,保持tab面板的高亮位置正确
    watch: {
      '$route' (to, from) {
        const path = to.path
        var tmpArr = path.split('/')
        if (tmpArr[1] === 'index') {
          this.handleTabChange(tmpArr[2])
        }
      }
    },
    methods: {
      handleTabChange (val) {
        this.activeTab = val
        this.$router.push({ path: '/index/' + val })
      }
    },
    computed: {
      ...mapGetters([
        'songList'
      ])
    }
  }
</script>
<style lang="less" scoped>
  @import "../assets/theme.less";
  .logo {/*logo之后可以要改成侧面弹出栏*/
    width: 25px;
    height: 20px;
    margin-left: ;
    background: url("../../static/logo.png") no-repeat left center;
    background-size: cover;
  }
  .view-tabs {
    background-color: #fff;
    color: rgba(0,0,0,.87);
    >.mu-tab-link {
      color: rgba(102,102,102,1);
    }
    >.mu-tab-active{
      color: @primaryColor;
    }
  }

  .fixed-bar {
    position: fixed;
    width: 100%;
    top:0;
    left: 0;
    z-index: 15;
  }
  .default-view {
    margin-top: 104px;
  }
  .view {
    margin-bottom: 2.3rem;
  }
</style>
