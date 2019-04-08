<template>
  <div id="get-posts">
    <div>
      <nav class="panel">
        <div class="panel-block">
          <p class="control has-icons-right">
            <input type="text" v-model="searchdata" class="input is-small" v-on:change="onChange" placeholder="search">
            <span class="icon is-small is-left">
              <i class="fas fa-search" aria-hidden="true"></i>
            </span>
          </p>
        </div>
      </nav>
    </div>
    <div>
      <aside class="menu">
        <ul class="menu-list">
          <a class="is-active" style="padding: 5px;" v-on:click="showPostList()">Posts</a>
          <ul>
            <li v-for="post in posts" v-bind:key="post.id" v-on:click="showCommentPostid(post)">
              <a style="padding: 7px;">{{ post.title }}</a>
            </li>
          </ul>
        </ul>
      </aside>
    </div>
  </div>
</template>

<script>

export default {
	data(){
		return {
      posts: [],
      searchdata: ''
		}
	},
	methods: {
    onChange: function (){
      var url = 'https://jsonplaceholder.typicode.com/posts' 
      var endpoint = ''
      if (this.searchdata != undefined && this.searchdata != ''){ 
        endpoint = '?title='+this.searchdata
      }
      this.$http
        .get(url+endpoint)
        .then(function(response) {
          this.posts = response.body.slice(0,10)
      })
    },
    showPostList(){
      this.$emit('showPostList', true);
    },
    showCommentPostid(data){
      this.$emit('showListComment', data);
    }
	},
	created() {
    this.$http
      .get('https://jsonplaceholder.typicode.com/posts')
      .then(function(response) {
        this.posts = response.body.slice(0,10)
      })
	}
}


</script>

<style lang='scss'>
  @import '../assets/bulma-docs.min.css';
</style>