<template>
  <div id="article">
    <!-- 数据未返回，显示此图 -->
    <div class="loading" v-if="isLoading">
      <img src="../assets/loading.png">
    </div>
    <div v-else>
      <div class="panel">
        <!-- 帖子标题 -->
        <div class="topic_header">
          <span class="topic_full_title"> {{post.title}}</span>
          <div class="changes">
            <span>发布于 {{post.create_at | formatDate}}</span>
            <span>作者 {{post.author.loginname}}</span>
            <span>{{post.visit_count}} 次浏览</span>
            <span> 来自 {{post | formatTopicTitle}}</span>
          </div>
          
        </div>
        <!-- 帖子内容 -->
        <div  class="topic_content">
          <div id="content" v-html="post.content"></div>
        </div>
        <!-- 回复 -->
        <div class="reply-content">
          <div class="reply-header">
            <span class="col_fade">{{post.reply_count}} 回复</span><!---->
          </div>
          

          <div class="reply_item  clear-fix" :class="{'riw-g': reply.ups.length > 2}" v-for="(reply, index) in post.replies" :key="reply.id">
            <!-- <div class="reply_item  clear-fix" v-for="(reply, index) in post.replies" :key="reply.id"> -->
              <div class="author_content">
                <router-link :to="{
                  name: 'use_info',
                  params:{
                    name: reply.author.loginname//需要传递的参数
                  }
                }">
                  <i href="" class="user_avatar">
                    <img :src="reply.author.avatar_url">
                  </i>
                </router-link>
                

                <div class="user_info">
                  <router-link :to="{
                    name: 'use_info',
                    params:{
                      name: reply.author.loginname//需要传递的参数
                    }
                  }">
                    <a class="dark reply_author" href="">{{reply.author.loginname}}</a>
                  </router-link>                  
                  <a class="reply_time" href="#5b99e957ce9d14c2254df9c7">{{index + 1}}楼• {{reply.create_at|formatDate}}</a>               
                </div>

                <div class="user_action">
                  <span>
                    <i class="fa" v-if="reply.ups.length>0"></i>
                    <i v-else></i>
                    <span class="up-count" v-if="reply.ups.length>0">{{reply.ups.length}}</span>
                    <span v-else></span>
                  </span>
                </div>
              </div>

              <div class="reply_content">
                <div id="content" v-html="reply.content"></div>
              </div>
            <!-- </div> -->
          </div>

          

        </div>
      </div>
    </div>
    
    
  </div>
</template>

<script>
export default {
  name: 'Article',
  data(){
    return {
      isLoading: false,
      post: {},//当前文章页所有内容
      name: 'x'
    }
  },
  methods:{
    getArticleData(){
      this.$http.get(`https://cnodejs.org/api/v1/topic/${this.$route.params.id}`)
      .then(res=>{
        if(res.data.success == true){
          this.isLoading = false;
          this.post = res.data.data
          // console.log(res.data.data)
        }
      })
      .catch(err=>{
        this.isLoading = true;
        console.log(err +'---无法获取帖子')
      })
    }
  },
  beforeMount(){
    this.isLoading = true;
    this.getArticleData();
  },
  watch:{//检测路由变化
    '$route'(to,from){
      this.getArticleData()//路由变化就执行这个方法
    }
  }
}
</script>

<style>
  @import url('../assets/markdown-github.css');
  .reply-content .riw-g{
        background-color: #f4fcf0;
  }
  /* .reply_item .riw-w{
        background-color: #fff;
  } */

  .reply-content{
    margin-top: 13px;
    font-size: 14px;
    color: #333;    
  }
  .reply-content #content{
    padding-left: 50px;
    margin: 0;
  }
  .reply-content #content >p{
    font-size: 15px;
    line-height: 1.7em;
    overflow: auto;
  }
  .reply-content #content p:first-child{
    margin-top: 0;
  }
  .reply-content #content p{
    margin-top: 15px;
    margin-bottom: 15px;
  }
  .reply-header{
    padding: 10px;
    background-color: #f6f6f6;
    border-radius: 3px 3px 0 0;
  }
  .reply-header .col_fade {
    color: #444;
  }
  .reply_item{
    position: relative;
    padding: 10px;
    background-color: #fff;
    border-top: 1px solid #f0f0f0;  
    font-size: 14px;
    overflow: hidden;
  }

  
  .reply_item:last-child{
    border-radius: 0 0 3px 3px;
  }
  .author_content .user_avatar {
    display: block;
    float: left;
  }
  .user_avatar img, .user_big_avatar img {
    width: 30px;
    height: 30px;
    border-radius: 3px;
    max-width: 100%;
    vertical-align: middle;
  }
  .user_info {
    margin-left: 10px;
    display: inline-block;
  }
  .user_info a.dark,.user_info a.dark:active,.user_info a.dark:link,.user_info a.dark:visited {
    color: #666;
    text-decoration: none;
  }
  .user_info a.dark{
    text-overflow: ellipsis;
    overflow: hidden;
  }
  .user_info .reply_author{
    font-size: 12px;
    font-weight: 700;
  }
  .user_info .reply_author:hover{
    color: #08c;
  }
  .reply_time {
    font-size: 11px;
    color: #08c;
    text-decoration: none;
    /* padding: 10px; */
  }
  .reply_time:hover{
    text-decoration: underline;
  }
  .user_action {
    float: right;
    margin-left: 20px;
    font-size: 15px;
  }
  .user_action .fa{
    font: normal normal normal 14px/1 iconfont;
    font-size: inherit;
    text-rendering: auto;
    -webkit-font-smoothing: antialiased;
    display: inline-block;
    cursor: pointer;
    opacity: .4;
    color: #000;
  }
  .user_action .fa:before{
    content: '\e6ea'
  }
  /* 帖子 */
  .topic_header{
    background-color: #fff;
    padding: 10px;
    border-radius: 3px 3px 0 0;
  }
  .topic_full_title {
    font-size: 22px;
    font-weight: 700;
    margin: 8px 0;
    display: inline-block;
    vertical-align: bottom;
    width: 75%;
    line-height: 130%;
  }
  .changes {
    font-size: 12px;
    color: #838383;
    overflow: hidden;
  }
  .changes>span>a{
    color: #838383;

  }
  .changes>span:before {
    content: "• ";
  }
  .topic_content{
    padding: 10px;
    border-top: 1px solid #e5e5e5;
    background-color: #fff;
    line-height: 2em;
    border-radius: 0 0 3px 3px;
    font-size: 14px;
  }
  
  .clear-fix{
    content: '';
    display: block;
    /* clear: both; */
  }
  
</style>
