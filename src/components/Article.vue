<template>
    <div class="article">
      <!--//如果正在加载显示此div-->
      <div class="isloading" v-if="isLoading">
        <img src="../assets/loading.gif" alt="加载中">
      </div>
      <div v-else>
        <div class="topic_header">
          <div class="top_title">{{post.title}}</div>
          <ul>
            <li>发布于:{{post.create_at|formDate}}</li>
            <li>作者:{{post.author.loginname}}</li>
            <li>{{post.visit_count}}次浏览</li>
            <li>来自{{post|tabFormatter}}</li>
          </ul>
          <div v-html="post.content" class="top_content"></div>
        </div>
        <div>
           <div class="topbar ">回复</div>
          <div v-for="(reply,index) in post.replies" class="replySec">
            <div class="replyUp">
              <router-link :to="{
              name:'user_info',
              params:{
                name:reply.author.loginname
                }
              }">
                <img :src="reply.author.avatar_url" alt="头像">
              </router-link>


                <span>{{reply.author.loginname}}</span>


              <span>
              {{index+1}}楼
               </span>
              <span v-if="reply.ups.length>0">
              👍{{reply.ups.length}}
               </span>
              <span v-else></span>
            </div>
            <p v-html="reply.content"></p>
          </div>
        </div>
      </div>
    </div>
</template>

<script>
    export default {
        name: "Article",
      data(){
          return{
            isLoading:false,
            post:{}//代表当前文章页的内容，属性
          }
      },
      methods:{
          getArticleData(){
            this.$http.get(`https://cnodejs.org/api/v1/topic/${this.$route.params.id}`)
              .then(res=>{
                // console.log(res)
                this.isLoading=false
                if(res.data.success)
                {this.post=res.data.data}
              })
              .catch(error=>console.log(error))
          }
      },
      beforeMount() {
          this.isLoading=true
          this.getArticleData()
      },
      watch:{
          '$route'(to,from){
            this.getArticleData()
          }
      }
    }
</script>

<style >
  @import url('../../node_modules/github-markdown-css/github-markdown.css');
  .replySec{
    border-bottom: 1px solid grey;
  }
  .replySec img{
    width: 30px;
  }
</style>
