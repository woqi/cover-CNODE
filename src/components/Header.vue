<template>
  <div class="header-wrap">
    <div class="header">
      <a class="brand" href="http://localhost:8080/#/">
        <img src="../assets/cnodejs_light.svg" alt="">
      </a>
      <ul class="pull-right">
        <li><a href="">首页</a></li>
        <li><a href="">新手入门</a></li>
        <li><a href="">API</a></li>
        <li><a href="">关于</a></li>
        <li><a href="">注册</a></li>
        <li><a href="">登录</a></li>
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
  .pull-right > li > a{
    text-decoration: none;
    padding: 10px 15px;
    color: #ccc;
    font-size: 13px;
  }
  .pull-right > li > a:hover{
    color: #fff;
  }
</style>



