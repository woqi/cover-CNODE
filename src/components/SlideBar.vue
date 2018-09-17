<template>
  <div id="sidebar">
    <div class="panel"><!--作者标签-->
      <div class="header">
        <span class="col_fade">作者</span>
      </div>
      <div class="inner">
        <div class="user_card">
          <router-link :to="{
                    name: 'use_info',
                    params:{
                      name: userinfo.loginname
                    }
                  }">
            <a href="" class="user_avatar"><!--用户头像-->
              <img :src="userinfo.avatar_url" alt="">
            </a>
          </router-link>    
          <router-link :to="{
                    name: 'use_info',
                    params:{
                      name: userinfo.loginname
                    }
                  }">
            <span class="dark user_name">{{userinfo.loginname}}</span><!--用户名-->       
          </router-link> 
          <span class="big">积分: {{userinfo.score}}</span><!--积分-->
        </div>
      </div>
    </div>
    <div class="panel"><!--作者最近主题-->
      <div class="header">
        <span class="col_fade">作者最近主题</span>
      </div>
      <div class="inner"> 
        <ul class="unstyled">
          <li v-for="list in recentTopicsBy5" :key="list.id">
            <div>
              <router-link :to="{
                name: 'post_content',
                params:{
                  id: list.id,
                  name: list.author.loginname
                }
              }">
                <span class="dark topic_title" href="">{{list.title}}</span>
              </router-link>
              
            </div>
          </li>   
      
        </ul>     
      </div>
    </div>
    <div class="panel"><!--作者最近回复话题-->
      <div class="header">
        <span class="col_fade">作者最近回复话题</span>
      </div>
      <div class="inner"> 
        <ul class="unstyled">
          <li v-for="list in replyTopicsBy5" :key="list.id">
            <div>
              <router-link :to="{
                name: 'post_content',
                params:{
                  id: list.id,
                  name: list.author,loginname
                }
              }">
                <span class="dark topic_title">{{list.title}}</span>
              </router-link>
              
            </div>
          </li>    
        </ul>     
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'SlideBar',
    data(){
        return {
          isLoading:false,
          userinfo: {}
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
      }
    },
    beforeMount(){
      this.isLoading = true;
      this.getUserInfoData();
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
  .panel {
    margin-bottom: 13px;
  }
  .panel .header {
    padding: 10px;
    background-color: #f6f6f6;
    border-radius: 3px 3px 0 0;
  }
  .col_fade {
    color: #444;
  }
  .inner{
    padding: 10px;
    border-top: 1px solid #e5e5e5;
    border-radius: 0 0 3px 3px;
    background-color: #fff;
    line-height: 2em;
  }
  .user_card .user_avatar {
    vertical-align: middle;
    margin-right: .5em;
  }
  .user_avatar img{
    width: 48px;
    height: 48px;
    border-radius: 3px;
    max-width: 100%;
    vertical-align: middle;
    border: 0;
  }
  .big {
    font-size: 14px;
    display: block;
  }
  .signature {
    font-style: italic;
    font-size: 13px;
  }
  .user_name {
    max-width: 120px;
    white-space: nowrap;
    display: inline-block;
    vertical-align: middle;  
    color: #778087;
  }
  .unstyled {
    padding-left: 10px;
    font-size: 14px;
    margin: 0;
  }
  .unstyled li {
    line-height: 2em;
  }
  .unstyled>li>a{
    color: #778087;
  }
  .user_info a.dark{
    text-overflow: ellipsis;
    overflow: hidden;
    text-decoration: none;
  }
  .topic_title {
    max-width: 270px;
    color: #778087;;
    text-overflow: ellipsis;
    white-space: nowrap;
    display: block;
    /* vertical-align: middle; */
    font-size: 14px;
    line-height: 30px;
    overflow: hidden;
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
