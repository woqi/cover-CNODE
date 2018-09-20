<template>
  <div class="header-wrap">
    <div class="header">
      <router-link :to="{
        name:'root'
        }">
        <a class="brand">
          <img src="../assets/cnodejs_light.svg" alt="">
        </a>
      </router-link>
      
      <ul class="pull-right">
        <li>
          <router-link :to="{
              name:'root'
              }">
              <a class="header-text" href="">首页</a>
          </router-link>
        </li>
        <li><a class="header-text" href="">新手入门</a></li>
        <li><a class="header-text" href="">API</a></li>
        <li><a class="header-text" href="">关于</a></li>
        <li><a class="header-text" href="">注册</a></li>
        <li><a class="header-text" href="">登录</a></li>
      </ul>
    </div>
    
  </div>
</template>

<script>
export default {
  name: 'Header'
}
</script>

<style scoped>
  .header-wrap{
    margin-bottom: 0;
    z-index: 9;
    width: 100%;
    height: 50px;
    position: relative;
    background: #444;
    font-size: 13px;
  }
  .header{
    background: 0 0;
    border-radius: 0;
    border: none;
    box-shadow: none;
    width: 90%;
    margin: auto;
    padding: 5px;
  }
  .brand{
    width: 120px;
    padding: 3px 20px;
    height: 34px;
    line-height: 34px;
    color: #ccc;
    font-weight: 700;
    font-size: 20px;
  }
  .brand img{
    width: 120px;
    vertical-align: initial;

  }
  .pull-right{
    list-style: none;
    float: right;
  }
  .pull-right > li{
    display: inline-block;  
  }
  .pull-right .header-text{
    text-decoration: none;
    padding: 10px 15px;
    color: #ccc;
    font-size: 13px;
  }
  .pull-right > li > a:hover{
    color: #fff;
  }
</style>



