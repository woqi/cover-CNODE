<template>
  <div class="post-list">
    <!-- 数据未返回，显示此图 -->
    <div class="loading" v-if="isLoading">
      <img src="../assets/loading.png">
    </div>
    <div class="post" v-else>
      <!-- tab版面 -->
      <div class="header">  
        <a href="/?tab=all" class="topic-tab current-tab">全部</a>
        <a href="/?tab=good" class="topic-tab ">精华</a>
        <a href="/?tab=share" class="topic-tab ">分享</a>
        <a href="/?tab=ask" class="topic-tab ">问答</a>
        <a href="/?tab=job" class="topic-tab ">招聘</a>
        <a href="/?tab=dev" class="topic-tab ">客户端测试</a> 
      </div>
    
    <!-- 帖子列表 -->
    <div class="topic_list" >
      <div class="cell" v-for="post in posts" :key="post.id">
        <!-- 头像 -->
        <router-link :to="{
            name: 'use_info',
            params:{
              name: post.author.loginname
            }
        }">
          <a class="user_avatar pull-left" href="">
            <img :src="post.author.avatar_url" alt="">
          </a>
        </router-link>
        
        <!-- 回复/浏览 -->
        <span class="reply_count pull-left">
          <span class="count_of_replies" title="回复数">
            {{post.reply_count}}
          </span>
          <span class="count_seperator">/</span>
          <span class="count_of_visits" title="点击数">
            {{post.visit_count}}
          </span>
        </span>
        <!-- 最终回复时间 -->
        <a class="last_time pull-right" href="">
          <!-- <img class="user_small_avatar" src=""> -->
          <!-- 此处回复人的头像需要到另一个组件拿 -->
          <!-- https://avatars2.githubusercontent.com/u/38970196?v=4&amp;s=120 测试图片-->
          <span class="last_active_time">{{post.last_reply_at|formatDate}}</span><!-- 过滤器过滤时间 -->
        </a>
        <!-- 置顶、精华、分享、问答、招聘 -->
        <div class="topic_title_wrap">
          <span :class="[{put_good:(post.good == true),put_top:(post.top == true),
          'topiclist-tab':(post.top == !true && post.top == !true)}]">{{post | formatTopicTitle}}</span>
          <!-- 标题 -->
          <router-link :to="{
            name: 'post_content',
            params:{
              id: post.id,
              name: post.author.loginname
            }
          }">
            <span class="topic_title">
              {{post.title}}
            </span>
          </router-link>
          
        </div>
      </div>
      <div class="cell clear-fix">
        <pagination @handleList="renderList"></pagination>
      </div>
    </div>
    </div>
    

    
  </div>
</template>

<script>
import pagination from './Pagination'
export default {
  name: 'PostList',
  data(){
    return{
      isLoading: false,
      posts: [],//代表页面列表数组
      postpage: 1
    }
  },
  components:{
    pagination
  },
  methods:{
    getData(){
      this.$http.get('https://cnodejs.org/api/v1/topics',{
        params:{//get请求需要写这个参数，post不需要
          page: this.postpage,
          limit: 40
        }
        
      })
      .then(res=>{
        this.isLoading = false;
        this.posts = res.data.data
        // console.log(res)
        // console.log(res.data.data)
      })
      .catch(err=>{
        console.log(err)
      })
    },
    renderList(value){
      this.postpage = value;
      this.getData();
      // console.log(value)
    }
  },
  beforeMount(){
    this.isLoading = true//加载成功前显示loading
    this.getData();//页面加载前获取数据
  }
}
</script>
  
<style scoped>
  .topic_list .cell:nth-child(1){
    border-top: none;
  }
  .cell{
    background: #fff;
    border-top: 1px solid #f0f0f0;
    position: relative;
    padding: 10px;
    font-size: 14px;
    overflow: hidden;
  }
  .cell:hover{
    background-color: #f6f6f6;
  }
  .pull-left {
    float: left;
  }
  .pull-right {
    float: right;
  }
  .user_avatar img{
      width: 30px;
      height: 30px;
      border-radius: 3px;
  }
  img{
    vertical-align: middle;
    border: 0;
  }
  .reply_count{
    width: 70px;
    display: inline-block;
    text-align: center;
  }
  .count_of_replies{
    color: #9e78c0;
  }
  .count_seperator {
    margin: 0 -3px;
    font-size: 10px;
  }
  .count_of_visits {
    font-size: 10px;
    color: #b4b4b4;
  }
  .last_time {
    font-size: 11px;
    display: inline-block;
    margin-left: 20px;
    color: #778087;
    
  }
  .user_small_avatar {
    height: 18px;
    width: 18px;
    vertical-align: middle;
    margin-right: .5em;
    border-radius: 3px;
  }
  .last_active_time {
    text-align: right;
    min-width: 50px;
    display: inline-block;
    white-space: nowrap;
  }
  .topic_title_wrapper {
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
  .topiclist-tab {
    background-color: #e5e5e5;
    color: #999;
    padding: 2px 4px;
    border-radius: 3px;
    -webkit-border-radius: 3px;
    -moz-border-radius: 3px;
    -o-border-radius: 3px;
    font-size: 12px;
  }
  .topic_title {
    color: #333;
    max-width: 70%;
    text-overflow: ellipsis;
    white-space: nowrap;
    display: inline-block;
    vertical-align: middle;
    font-size: 16px;
    line-height: 30px;
  }
  .topic_title:hover{
    text-decoration: underline;
  }
  .header {
    padding: 10px;
    background-color: #f6f6f6;
    border-radius: 3px 3px 0 0;
  }
  .topic-tab {
    margin: 0 10px;
    color: #80bd01;
  }
  .topic-tab:hover {
    color: rgb(5, 79, 116);
  }
  .current-tab {
    background-color: #80bd01;
    color: #fff;
    padding: 3px 4px;
    border-radius: 3px;
  }
  .clear-fix{
    content: '';
    display: block;
    clear: both;
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