<template>
  <div  class="pagination">
    <button @click="changeBtn">首页</button>
    <button @click="changeBtn">上一页</button>
    <button v-if="jduge" class="pagebtn">......</button>
    <button v-for="btn in pagebtns" :key="btn" 
    @click="changeBtn(btn)"
    :class="[{currentPage:btn == currentPage},'pagebtn']">
      {{btn}}
    </button>
    <button @click="changeBtn">下一页</button>
</div>
</template>

<script>
import $ from 'jquery'
  export default {
    name: 'Pagination',
    data(){
      return{
        pagebtns: [1,2,3,4,5,'......'],//储存中间显示内容
        currentPage: 1,//当前选中的页码
        jduge: false
      }
    },
    methods:{
      changeBtn(page){
        //点击上下页
        if(typeof page != 'number'){
          switch(page.target.innerText){
            case '上一页':
              $('button.currentPage').prev().click();
            break;
            case '下一页':
              $('button.currentPage').next().click();
            break;
            case '首页':
              this.pagebtns = [1,2,3,4,5,'......'];
              this.changeBtn(1);
            break
          }
          return;
        }

        if(page > 4){
          this.jduge = true;
        }else{
          this.jduge = false;          
        }

        this.currentPage = page;
        if(page == this.pagebtns[4]){
          this.pagebtns.shift();//移除第一个
          this.pagebtns.splice(4,0,this.pagebtns[3]+1);//添加最后一个
        }else if(page == this.pagebtns[0] && page !== 1){
          this.pagebtns.unshift(this.pagebtns[0]-1);//第一个数字加一
          this.pagebtns.splice(5,1);//最后一个减一个
        }

        this.$emit('handleList',this.currentPage)
      }
    }
  }

</script>
<style scoped>
  .pagination {
      height: 40px;
      margin-top: 10px;
  }
  ul{
    padding: 0;
    margin: 0;
    display: inline-block;
  }
  button {
    display: block;
    float: left;
    background-color: #fff;
    border: 1px solid #ddd;
    color: #778087;
    border-radius: 3px;
    outline: none;
    line-height: 20px;
    cursor: pointer;
    padding: 0 2px;
    width: 50px;
    text-align: center;
    margin-left: 3px;
  }

  .pagebtn {
    width: 40px;
    margin: 0 4px;
  }

  .currentPage {
    color: white;
    background-color: rgb(190, 190, 190);
  }

</style>
