<template>
<div>
  <div class="sidenav">
    <get-users v-on:showUserList="showUserList($event)" v-on:showPostByUId="showPostbyUserId($event)"></get-users> 
    <get-posts v-on:showPostList="showPostList($event)" v-on:showListComment="showListComment($event)"></get-posts>
  </div>
  <div class="content">
    <div v-show="showUsers">
      <list-users></list-users>
    </div>
    <div v-if="showPosts || showPostbyId">
      <list-posts v-bind:UserId="userdata"></list-posts>
    </div>
    <div v-if="showComment">
      <list-comment v-bind:PostId="postdata"></list-comment>
    </div>
  </div>
</div>

</template>

<script>
import getUsers from './components/getUsers.vue';
import getPosts from './components/getPosts.vue';
import listUsers from './components/listUsers.vue';
import listPosts from './components/listPosts.vue';
import listComment from './components/listComment.vue';

export default {
  name: 'app',
  components: {
    'get-users': getUsers,
    'get-posts': getPosts,
    'list-users': listUsers,
    'list-posts': listPosts,
    'list-comment': listComment

  }, 
  data(){
    return{
      showUsers: true,
      showPosts: false,
      showPostbyId: false,
      showComment: false,
      userdata: {}
    }
    
  },
  methods: {
    showDefault(){
      this.showUsers = false
      this.showPosts = false
      this.showPostbyId = false
      this.showComment = false
    },
    showUserList(data){
      this.showDefault()
      this.showUsers = data;
    },
    showPostList(data){
      this.showDefault()
      this.showPosts = data;
      this.userdata = {}
    },
    showPostbyUserId(data){
      this.showDefault()
      this.showPostbyId = true;
      this.userdata = data
    },
    showListComment(data){
      this.showDefault()
      this.showComment = true;
      this.postdata = data
    }
  }
}
</script>


// <style lang='scss'>
//   @import './assets/bulma-docs.min.css';
// </style>
<style>


/* Style the side navigation */
.sidenav {
  height: 100%;
  width: 230px;
  position: fixed;
  z-index: 1;
  top: 0;
  left: 0;
  overflow-x: hidden;
}


/* Side navigation links */
.sidenav a {
  padding: 16px;
  text-decoration: none;
  display: block;
}

/* Change color on hover */
.sidenav a:hover {
  background-color: #ddd;
  color: black;
}

.content {
  margin-left: 230px;
  padding-left: 20px;
  margin-top: 50px;
}
</style>
