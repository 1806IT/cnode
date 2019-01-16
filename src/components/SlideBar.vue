<template>
    <div class="autherinfo">
      <div class="authersummay">
        <div class="topbar">作者</div>
        <router-link :to="{
        name:'user_info',
        params:{
          name:userinfo.loginname
        }
        }">
          <img :src="userinfo.avatar_url" alt="">
        </router-link>
      </div>
      <div class="recent_topics">
        <div class="topbar">作者最近主题</div>
        <ul>
          <li v-for="list in topcilimitby5">
            <router-link :to="{
            name:'post_content',
            params:{
              id:list.id,
              name:list.author.loginname
            }
            }">
              {{list.title}}
            </router-link>

          </li>
        </ul>
      </div>
      <div class="recent_replies">
        <div class="topbar">作者最近回复</div>
        <ul>
          <li v-for="list in replieslimitby5">
            <router-link :to="{
            name:'post_content',
            params:{
              id:list.id,
              name:list.author.loginname
            }
            }">
              {{list.title}}
            </router-link>
          </li>
        </ul>
      </div>

    </div>
</template>

<script>
    export default {
        name: "SlideBar",
      data(){
          return{
            userinfo:{}
          }
      },
      methods:{
          getData(){
            this.$http.get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
              .then(rest=>{
                console.log(1)
                this.userinfo=rest.data.data
                console.log(this.userinfo)
              })
              .catch(error=>console.log(error))
          }
      },
      computed:{
          topcilimitby5(){
            // console.log(this.userinfo.recent_topics)
            // console.log(typeof this.userinfo.recent_topics)
            // console.log(this.userinfo.recent_topics.length)
            // return this.userinfo.recent_topics
            if(this.userinfo.recent_topics){
              return this.userinfo.recent_topics.slice(0,5)
            }
           },
        replieslimitby5(){
          if(this.userinfo.recent_replies){
            return this.userinfo.recent_replies.slice(0,5)
          }
        }
      },
      beforeMount(){
          this.getData()
      }
    }
</script>

<style scoped>
  .autherinfo{
    border: 1px solid red;
    display: flex;
    flex-direction: column;
  }
</style>
