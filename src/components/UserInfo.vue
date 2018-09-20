<template>
  <div id="UserInfo">
    <!-- 数据未返回，显示此图 -->
    <div class="loading" v-if="isLoading">
      <img src="../assets/loading.png">
    </div>
    <div v-else>
      <div class="panel">
        <div class="header">
          <ul class="breadcrumb">
            <li><a href="/">主页</a><span class="divider">/</span></li>
          </ul>
        </div>
        <div class="inner">
          <div class="user_big_avatar">
            <img :src="userinfo.avatar_url" class="user_avatar" >
          </div>
          <a class="dark">{{userinfo.loginname}}</a>
          <div class="user_profile">
            <ul class="unstyled">
              <span class="big">{{userinfo.score}}</span> 积分  
              <li>
                <a class="dark" href="s" target="_blank">
                  <span class="collect-topic-count" v-for="(collect, num) in topicollect" :key="collect.id" >{{num+1}}</span>个话题收藏                    
                </a>
              </li>          
              <li>
                <i class="fa fa-home"></i>
                <a class="dark" :href="[`https://github.com/${this.userinfo.githubUsername}`]">https://github.com/{{userinfo.githubUsername}}</a>
              </li>           
              <li>
                <i class="fa fa-github"></i>
                <a class="dark" :href="[`https://github.com/${this.userinfo.githubUsername}`]">
                  {{userinfo.githubUsername}}
                </a>
              </li> 
            </ul>
          </div>
          <p class="col_fade">注册时间 {{userinfo.create_at | formatDate}}</p>
          
        </div>
      </div>
      <div class="panel recent-topics">
        <div class="header">
          <ul class="breadcrumb">
            <span class="col_fade">最近创建的话题</span>
          </ul>
        </div>
        <div class="cell"  v-for="topics in recentTopicsBy5" :key="topics.id">
          <a class="user_avatar pull-left" href="">
            <img :src="topics.author.avatar_url">
          </a>
          <!-- <span class="reply_count pull-left">
            <span class="count_of_replies">
              40
            </span>
            <span class="count_seperator">/</span>
            <span class="count_of_visits">
              2553
            </span>
          </span>  此处需要到另一个页面调用 -->
          <a class="last_time pull-right" href="">
            <!-- <img class="user_small_avatar" src="https://avatars1.githubusercontent.com/u/43138302?v=4&amp;s=120">
             此处需要到另一个页面调用-->
            <span class="last_active_time">{{topics.last_reply_at | formatDate}}</span>
          </a>
          <div class="topic_title_wrapper">  
            <!-- <span class="put_top">置顶</span> 
            此处需要到另一个页面调用-->
            <router-link :to="{
              name: 'post_content',
              params:{
                id: topics.id//此处是传到路由上的
              }
              
            }">
              <span class="topic_title" href="">
                {{topics.title}}
              </span>
            </router-link>
            
          </div>
        </div>
      </div>
      <div class="panel">
        <div class="header">
          <ul class="breadcrumb">
            <span class="col_fade">最近参与的话题</span>
          </ul>
        </div>
        <div class="cell" v-for="replies in replyTopicsBy5" :key="replies.id">
          <a class="user_avatar pull-left" href="">
            <img :src="replies.author.avatar_url">
          </a>
          <!-- <span class="reply_count pull-left">
            <span class="count_of_replies">
              {{post.reply_count}}20
            </span> -->
            <!-- <span class="count_seperator">/</span> -->
            <!-- <span class="count_of_visits"> -->
              <!-- 2553 -->
            <!-- </span> -->
          <!-- </span>   此处需要到另一个页面去调用--> 
          <a class="last_time pull-right" href="">
            <!-- <img class="user_small_avatar" src="https://avatars1.githubusercontent.com/u/43138302?v=4&amp;s=120"> -->
            <span class="last_active_time">{{replies.last_reply_at | formatDate}}</span>
          </a>
          <div class="topic_title_wrapper">  
            <!-- <span class="put_top">置顶</span>
            此处需要到另一个页面去调用 -->
            <router-link :to="{
              name: 'post_content',
              params:{
                id: replies.id//此处是传到路由上的
              }
              
            }">
              <span class="topic_title" href="">
                {{replies.title}}
              </span>
            </router-link>
          </div>
        </div>
      </div>
    </div>
    
  </div>
</template>

<script>
  export default {
    name: 'UserInfo',
    data(){
      return {
        isLoading: false,
        userinfo: {},//当前文章页所有内容
        topicollect: {},//收藏的帖子
        // post: {}
        name:'x'
      }
    },
    methods:{
      getUserInfoData(){
        this.$http.get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
        .then(res=>{
          if(res.data.success == true){
            this.isLoading = false;
            this.userinfo = res.data.data
            // console.log(res.data.data)
            
          }
        })
        .catch(err=>{
          this.isLoading = true;
          console.log(err +'---无法获取帖子')
        })
      },
      getUserTopicCollectData(){
        this.$http.get(`https://cnodejs.org/api/v1/topic_collect/${this.$route.params.name}`)
        .then(res=>{
          if(res.data.success == true){
            this.topicollect = res.data.data
            // console.log(res.data.data)
          }
        })
        .catch(err=>{
          console.log(err +'---无法获取帖子')
        })
      },
    },
    beforeMount(){
      this.isLoading = true;//加载成功之前显示加载动画
      this.getUserInfoData();//在页面加载之前获取数据
      this.getUserTopicCollectData()
    },
    computed:{
      recentTopicsBy5(){
        if(this.userinfo.recent_topics){
          return this.userinfo.recent_topics.slice(0,5);
        }
      },
      replyTopicsBy5(){
        if(this.userinfo.recent_replies){
          return this.userinfo.recent_replies.slice(0,5);
        }
      }
    }
  }
</script>
<style scoped>
  .more{
    display: block;
  }
  ul,li{
    margin: 0;
    padding: 0;
  }
  .divider{
    color: #ccc;
  }
  .panel {
    margin-bottom: 13px;
  }
  .panel .header {
    padding: 10px;
    background-color: #f6f6f6;
    border-radius: 3px 3px 0 0;
  }
  .breadcrumb {
    padding: 0;
    margin: 0;
    border: none;
    background: 0 0;
  }
  .breadcrumb>li{
    text-shadow: none;
    line-height: 20px;
  }
  .breadcrumb>li>a{
    color: #80bd01;
  }
  .inner{
    padding: 10px;
    border-top: 1px solid #e5e5e5;
    border-radius: 0 0 3px 3px;
    background-color: #fff;
    line-height: 2em;
  }
  .user_big_avatar {
    float: left;
    margin-right: 10px;
  }
  .user_big_avatar img {
    width: 40px;
    height: 40px;
    border-radius: 3px;
  }
  .dark{
    color: #778087;
    text-overflow: ellipsis;
    overflow: hidden;
  }
  .user_profile {
    margin-top: 20px;
    clear: left;
  }
  .unstyled {
    padding-left: 10px;
    font-size: 14px;
  }
  .unstyled li {
    line-height: 2em;
  }
  .unstyled>li>a{
    color: #778087;
  }
  .fa {
    width: 1.28571429em;
    font-size: 1.33333333em;
    line-height: .75em;
    /* vertical-align: -15%; */
    font: normal normal normal 14px iconfont;
    text-rendering: auto;
    color: #000;
    opacity: .4;
    text-align: center;
    -webkit-font-smoothing: antialiased;
  }
  .fa-home:before{
    content: '\e617';
    font-size: 1.2em;
  }
  .fa-map-marker:before{
    content: '\e611';
    font-size: 1.4em;
    vertical-align: -15%;
  }
  .fa-github:before{
    content: '\e622';
    font-size: 1.2em;
  }
  .col_fade{
    color: #444;
  }
  .panel .cell {
    padding: 10px;
    background: #fff;
    border-top: 1px solid #f0f0f0;
  }
  .pull-left {
    float: left;
  }
  .pull-right{
    float: right;
  }
  .user_avatar img{
    width: 30px;
    height: 30px;
    border-radius: 3px;
  }
  .cell .reply_count {
    width: 70px;
    display: inline-block;
    text-align: center;
  }
  .cell .count_of_replies {
    color: #9e78c0;
  }
  .cell .count_seperator {
    margin: 0 -3px;
    font-size: 10px;
  }
  .cell .count_of_visits {
    font-size: 10px;
    color: #b4b4b4;
  }
  .last_time {
    font-size: 11px;
    display: inline-block;
    margin-left: 20px;
    color: #777;
  }
  .last_time .user_small_avatar {
    height: 18px;
    width: 18px;
    vertical-align: middle;
    margin-right: .5em;
    border-radius: 3px;
  }
  .last_time .last_active_time {
    text-align: right;
    min-width: 50px;
    display: inline-block;
    white-space: nowrap;
  }
  .topic_title_wrapper{
    white-space: nowrap;
    text-overflow: ellipsis;
  }
  .put_good, .put_top {
    background: #80bd01;
    padding: 2px 4px;
    border-radius: 3px;
    -webkit-border-radius: 3px;
    -moz-border-radius: 3px;
    -o-border-radius: 3px;
    color: #fff;
    font-size: 12px;
  }
  .topic_title_wrapper .topic_title {
    max-width: 70%;
    color: #08c;
    text-overflow: ellipsis;
    white-space: nowrap;
    display: inline-block;
    vertical-align: middle;
    font-size: 16px;
    line-height: 30px;
    margin-left: 25px/*因为没有请求到浏览量和回复量*/;
    /* topic_title */
  }
  .topic_title_wrapper .topic_title:hover{
    text-decoration: underline;
  }
  .loading{
    animation: loading-icon 1s infinite;
    background-size: 20px 20px;
    width: 20px;
    height: 20px;
    margin: 50px auto;
  }
  @keyframes loading-icon {
    from { transform: rotate(0deg) }
    8.32% { transform: rotate(0deg) }
    8.33% { transform: rotate(30deg) }
    16.65% { transform: rotate(30deg) }
    16.66% { transform: rotate(60deg) }
    24.99% { transform: rotate(60deg) }
    25% { transform: rotate(90deg) }
    33.32% { transform: rotate(90deg) }
    33.33% { transform: rotate(120deg) }
    41.65% { transform: rotate(120deg) }
    41.66% { transform: rotate(150deg) }
    49.99% { transform: rotate(150deg) }
    50% { transform: rotate(180deg) }
    58.32% { transform: rotate(180deg) }
    58.33% { transform: rotate(210deg) }
    66.65% { transform: rotate(210deg) }
    66.66% { transform: rotate(240deg) }
    74.99% { transform: rotate(240deg) }
    75% { transform: rotate(270deg) }
    83.32% { transform: rotate(270deg) }
    83.33% { transform: rotate(300deg) }
    91.65% { transform: rotate(300deg) }
    91.66% { transform: rotate(330deg) }
    99.99% { transform: rotate(330deg) }
  }
</style>
