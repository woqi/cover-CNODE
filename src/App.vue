<template>
  <div id="app">
    <Header/>
    <div class="main">
      <router-view name="slidebar"></router-view>
      <router-view name="main"></router-view>
      <!--这个main是路由里的main-->
      <!-- <PostList/> -->
      
    </div>
  </div>
</template>

<script>
import Header from './components/Header'
import PostList from './components/PostList'


export default {
  name: 'App',
  data(){
      return {
        
      }
    },
  components:{
    Header,
    PostList
  }
}
</script>

<style>
  body{
    font-family: "Helvetica Neue","Luxi Sans","DejaVu Sans",Tahoma,"Hiragino Sans GB",STHeiti,sans-serif!important;
    background-color: #e1e1e1;
    margin: 0;
  }
  .main{
    width: 90%;
    max-width: 1400px;
    min-width: 960px;
    margin: 15px auto;
    min-height: 400px;
  }
  a{
    text-decoration: none;
  }
  div{
    margin: 0;
  }
  ul,li{
    list-style: none;

  }
  img{
    max-width: 100%;
    vertical-align: middle;
    border: 0;
  }

  @font-face {
    font-family: 'iconfont';  /* project id 836537 */
    src: url('//at.alicdn.com/t/font_836537_u6jgjfoktxh.eot');
    src: url('//at.alicdn.com/t/font_836537_u6jgjfoktxh.eot?#iefix') format('embedded-opentype'),
    url('//at.alicdn.com/t/font_836537_u6jgjfoktxh.woff') format('woff'),
    url('//at.alicdn.com/t/font_836537_u6jgjfoktxh.ttf') format('truetype'),
    url('//at.alicdn.com/t/font_836537_u6jgjfoktxh.svg#iconfont') format('svg');
  }
  #sidebar {
    width: 290px;
    font-size: 14px;
    float: right;
    margin-bottom: 20px;
  }
  #article {
    padding: 0;
    margin-right: 305px;
  }
  
</style>
