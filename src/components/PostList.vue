<template>
    <div>
      <div class="loading" v-if="isLoading">
        <!--在数据未返回的时候，显示这个loading的图片-->
        <img src="../assets/loading.gif" alt="下载中">
      </div>
      <!--代表我们的主题帖子列表-->
      <div class="posts" v-else>
        <ul>
          <li>
            <div class="topbar">
              <span>全部</span>
              <span>精华</span>
              <span>分享</span>
              <span>问答</span>
              <span>招聘</span>
            </div>
          </li>
          <li v-for="post in this.posts" >
            <!--头像-->
            <img :src="post.author.avatar_url" alt="头像">
            <!--回复/浏览-->
            <span>
              <span class="replay_count">{{post.reply_count}}</span>
              <span class="visit_count">/{{post.visit_count}}</span>
            </span>
            <!--帖子分类-->
            <span :class="[{
            // spanpost_tab_share:(post.tab=='share'),
            // post_tab_ask:(post.tab=='ask'),
            // post_tab_job:(post.tab=='job'),
            topiclist_tab:(post.good!=true&&post.top!=true),
            put_good:(post.good===true),
            put_top:(post.top==true)
            }]">
              <span>{{post|tabFormatter}}</span>
            </span>
            <!--标题-->
            <router-link :to="{
            name:'post_content',
            params:{
             id:post.id,
             name:post.author.loginname
            }
            }">
              <div class="title">
                {{post.title}}
              </div>
            </router-link>

            <!--最终回复时间-->
            <span class="last-reply">
              {{post.last_reply_at|formDate}}
            </span>
          </li>
          <li>
            <!--分页-->
            <Pagination @handleList="renderList"></Pagination>
          </li>
        </ul>
      </div>
    </div>
</template>

<script>
  import Pagination from './Pagination'
    export default {
      name: "PostList",
      data (){
        return {
          isLoading:false,//加载成功去除动画
          posts:[],//代表页面中的数组
          postpage:1
        }
      },
      components:{
        Pagination
      },
      filters:{

      },
      methods:{
        renderList(value){
          console.log('重新渲染');
          console.log(value);
          this.postpage=value
          this.getData()
        },
        getData(){
          this.$http.get('https://cnodejs.org/api/v1/topics',{
            params:{
              page:this.postpage,
              limit:20
            }
          })
            .then(res=>{
              this.isLoading =false
              this.posts=res
             // console.log(res)
              this.posts=res.data.data
              // console.log(this.posts)
            }).catch(err=>{console.log(err)})
        }
      },
      beforeMount() {
        this.isLoading=true//页面加载成功前的动画
        this.getData()//页面加载成功前获取数据
      }
    }
</script>

<style scoped>
  ul{
    list-style:none;
  }
ul li:not(:first-child){
  padding: 9px;
  font-size: 15px;
  font-family: "Helvetica Neue","Droid Sans",Tahoma,"Hiragino Sans GB",STHeiti;
  font-weight: 400;
  background-color: white;
  color: #333;
  border:1px solid #f0f0f0;
}
  ul li:not(:first-child):hover{
    background: #f1f3f4;

  }
  ul li:first-child{
    background: #e4e4e4;
    border-radius: 3px;
    padding:2px;
    font-size: 14px;
    color: #a8c023;
  }

  .topbar>span:hover{
    background: grey;
    color: #d7174a;
  }
  .loading  {
    text-align: center;
  }
  .loading>img{
    width: 200px;
    margin:0 auto;
  }
img {
  width: 40px;
}
  .title{
    display: inline-block;
    max-width:75%;
    overflow: paged-x;
    font-size: 14px;
  }
  .last-reply{
    float: right;
    margin-top: 10px;
    font-size: 10px;
  }
  .put_good, .put_top{
    display: inline-block;
    height: 16px;
    border-radius: 3px;
    padding: 2px 4px;
    font-size: 12px;
    margin-right: 10px;
    color: #ffffff;
  }
  .put_good{
    background:gold;
  }
  .put_top{
    background:darkred;
  }
  .topiclist_tab{
    display: inline-block;
    height: 16px;
    background:darkgrey;
    font-size: 12px;
    border-radius: 2px;
  }

</style>
