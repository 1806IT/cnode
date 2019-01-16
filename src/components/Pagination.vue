<template>
    <div class="pagination">
      <button @click="changeBtn">首页</button>
      <button @click="changeBtn">上一页</button>
      <button @click="changeBtn" v-if="(pagebtn[0]>1)">……</button>
      <button v-for="(btn,index) in pagebtn"
              @click="changeBtn(btn)"
              :class="[{currentPage:btn==currentPage},'pagebtn']">
        {{btn}}
      </button>
      <button @click="changeBtn">下一页</button>
    </div>
</template>

<script>
  import $ from 'jquery'
    export default {
        name: "Pagination",
      data(){
          return{
            pagebtn:[1,2,3,4,5,'……'],
            currentPage:1,
            page:''
          }
      },
      methods:{
        changeBtn(page){
          // 如果没传参数进来进来那么page就是原生的事件，就不是number类型，据此判断用户点击的为上一页，下一页，首页
          if(typeof page!= 'number'){
            console.log(page)
            console.log(page.target);
            console.log(page.target.innerText);
            switch (page.target.innerText) {
              case '上一页'|'……':
                $('button.currentPage').prev().click();break;
              case '下一页':
                $('button.currentPage').next().click();break;
              case '首页':
                this.pagebtn=[1,2,3,4,5,'……'];
                this.currentPage=1
                this.changeBtn(1)
                break;
              default:break
            }
            return
          }
          console.log('点击了');
          this.currentPage=page
          if(page===this.pagebtn[4]){
            this.pagebtn.shift()//移除第一个
            this.pagebtn.splice(4,0,this.pagebtn[3]+1)//添加最后一个
          }else if(page==this.pagebtn[0]&&page!==1) {
            //现在第一个位置加一个
            this.pagebtn.unshift(this.pagebtn[0]-1)
            //移除最后一个
            this.pagebtn.splice(5,1)
          }
          this.$emit('handleList',this.currentPage)
        }
      }
    }
</script>

<style scoped>
.pagebtn{
  background:#fff;
}
  .currentPage{
    background:#aaaaaa;
  }
</style>
