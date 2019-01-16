<template>
    <div class="userInfo">
      <div class="loading" v-if="isloading">
        <img src="../assets/loading.gif" alt="下载中">
      </div>
      <div class="userInfomation" v-else>
        <section>
          <img :src="userinfo.avatar_url" alt="头像">
          <span>{{userinfo.loginname}}</span>
          <p>
            {{userinfo.score}}积分
          </p>
          <p>
            注册时间{{userinfo.create_at|formDate}}
          </p>
        </section>
        <div class="replies">
          <p>回复的主题</p>
          <ul>
            <li v-for=" item in userinfo.recent_replies">
              <router-link :to="{
              name:'post_content',
              params:{id:item.id}
              }">
                {{item.title}}
              </router-link>
            </li>
          </ul>
        </div>
        <div class="topics">
          <p>创建的主题</p>
          <ul>
            <li v-for="item in userinfo.recent_topics">
              <router-link :to="{
              name:'post_content',
              params:{id:item.id}
              }">
                {{item.title}}
              </router-link>
            </li>
          </ul>
        </div>
      </div>
    </div>
</template>

<script>
    export default {
        name: "UserInfo",
      data(){
          return{
            isloading:false,
            userinfo:{}
          }
      },
      methods:{
          getData(){
            this.$http.get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
              .then(res=>{
                console.log(1)
                this.isloading=false
                console.log(res)
                this.userinfo=res.data.data
              })
              .catch(error=>console.log(error))
          }
      },
      beforeMount() {
          this.isloading=true
          console.log(2)
        this.getData()
      }
    }
</script>

<style scoped>
img {
  width: 30px;
}
</style>
