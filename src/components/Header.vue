<template>
    <div class="header">
      <img src="../assets/logoq.png" alt="">
      <ul>
        <li><a href="#">首页</a></li>
        <li><a href="#">新手入门</a></li>
        <li><a href="#">API</a></li>
        <li><a href="#">关于</a></li>
        <li><a href="#">注册</a></li>
        <li><a href="#">登陆</a></li>

      </ul>
    </div>
</template>

<script>
    export default {
        name: "Header"
    }
</script>

<style scoped>
  .header{
    background:#655f60;
    height:50px;
  }
  img{
    max-width:120px;
    height: 50px;
  }
  ul{
    list-style:none;
    float:right;
    margin:4px;
  }
  li{
    display:inline-block;
    padding:10px 15px;
  }
  a{
    text-decoration:none;
    font-size: 14px;
    color: #ccc;

    
  }
</style>
