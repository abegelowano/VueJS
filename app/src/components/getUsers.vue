<template>
  <div id="get-users" style="height: 420px;">
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
          <a class="is-active" style="padding: 5px;" v-on:click="showUserList();"><strong>Users</strong></a>
          <ul>
            <li v-for="user in users" v-bind:key="user.id" v-on:click="showPostByUserid(user)">
              <a style="padding: 7px;">{{user.name}}</a>
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
      users: [],
      searchdata: ""
		}
	},
	methods: {
    onChange: function (){
      var url = 'https://jsonplaceholder.typicode.com/users'
      var endpoint = '' 
      if (this.searchdata != undefined && this.searchdata != ''){ 
        endpoint = '?username='+this.searchdata
      }
      this.$http
        .get(url+endpoint)
        .then(function(response) {
          this.users = response.body
      })
    },
    showUserList(){
      this.$emit('showUserList', true);
    },
    showPostByUserid(data){
      this.$emit('showPostByUId', data);
    }
	},
	created() {
    this.$http
      .get('https://jsonplaceholder.typicode.com/users')
      .then(function(response) {
        this.users = response.body
      })
	}
}


</script>
.h1{
  font-family: Open sans;
}
<style lang='scss'>
  @import '../assets/bulma-docs.min.css';
</style>
