<template>
	<div id="list-posts">
    <h4><center><strong>Posts</strong></center></h4>
    <h6 v-if="UserId.name">User: {{UserId.name}}</h6>
    <!-- okay Modal -->
    <div class="modal is-active" v-show="showOkayModal">
      <div class="modal-background"></div>
      <div class="modal-content">
        
        <header class="modal-card-head">
          <p class="modal-card-title">{{modal.msg}}</p>
          <button class="delete " aria-label="close" v-on:click="showOkayModal=false"></button>
        </header>
        <footer class="modal-card-foot">
            <button class="button" v-on:click="showOkayModal=false">okay</button>
        </footer>
      </div>
    </div>

    <!-- create/edit Modal -->
    <div class="field is-grouped is-grouped-multiline"> 
      <a class="button is-primary" v-on:click="createPost()">
        <strong>Create Post </strong>
      </a>
    </div>
    <div class="modal is-active" v-show="showCreateModal">
      <div class="modal-background"></div>
      <div class="modal-content">
        
        <header class="modal-card-head">
          <p class="modal-card-title">{{modal.title}}</p>
          <button class="delete" aria-label="close" v-on:click="cancel()"></button>
        </header>
        <section class="modal-card-body">

          <h6 style="padding-top: 10px;">Title</h6>
          <input type="text" placeholder="Title" class="input" v-model="createpost.title">
    
          <h6 style="padding-top: 10px;">Body</h6>
          <textarea placeholder="Body" class="textarea" v-model="createpost.body"></textarea>

        </section>
        <footer class="modal-card-foot">
          <button class="button is-success" v-on:click ="post_patch(createpost)">Save</button>
          <button class="button" v-on:click="cancel()">Cancel</button>
        </footer>
      </div>
    </div>

    <!-- list -->
    <table >
      <thead>
        <th width="5%">User Id</th>
        <th width="20%">Title</th>
        <th width="20%">Body</th>
        <th width="15%" style="text-align:center">Action</th>
      </thead>
      <tbody>
        <tr v-for="(post,index) in posts" v-bind:key="post.id" >
          <td width="5%">{{ post.userId}}</td>
          <td width="20%">{{post.title}}</td>
          <td width="20%">{{post.body}}</td>
          <td width="15%">    
            <div style="text-align:center">
              <a class="button is-primary" v-on:click="editPost(post);showCreateModal=true">
                <strong>Edit</strong>
              </a>&nbsp;&nbsp;
              <a class="button is-primary" v-on:click="deletePost(post,index)">
                <strong>Delete</strong>
              </a>
            </div>

          </td>
        </tr>
      </tbody>
    </table>
	</div>
</template>
	
<script>

export default {
  props:[
    'UserId'
  ],
	data(){
		return {
      modal: {
        title: "",
        msg: "",
        action: ""
      },
      showCreateModal: false,
      showOkayModal: false,
      posts: [],
      createpost: {
        'userId': 1,
        'title': '',
        'body':''
      }
		}
	},
	methods: {
    deletePost(post,index){
      this.$http
      .delete('https://jsonplaceholder.typicode.com/posts/'+post.id)
      .then(function() {
        this.showOkayModal= true;
        this.modal.msg = "Successfully Deleted!"
        this.posts.splice(index, 1);
      })
    },
    post_patch(post){
      if (this.modal.action==="POST"){
        this.$http
        .post('https://jsonplaceholder.typicode.com/posts',this.createpost)
        .then(function(response) {
          this.posts.push(response.body);
          this.showCreateModal = false;
          this.showOkayModal= true;
          this.modal.msg = "Successfully Saved!"
          this.createposts = {'userId': 1,'title': '','body':''}

        });
      } else if (this.modal.action==="PATCH"){
        this.$http
        .patch('https://jsonplaceholder.typicode.com/posts/'+post.id,post)
        .then(function() {
          this.showCreateModal = false;
          this.showOkayModal= true;
          this.modal.msg = "Successfully Updated!"
          this.createpost = {'userId': 1,'title': '','body':''}

        });
      }

    },
    createPost(){
      this.showCreateModal = true
      this.modal.title = 'Create New Post'
      this.modal.action = "POST"
    },
    editPost(post){
      this.showCreateModal = true
      this.createpost = post
      this.modal.title = 'Edit Post'
      this.modal.action = "PATCH"
    },
    cancel(){
      this.showCreateModal=false
      this.createpost = {'userId': 1,'title': '','body':''}
    },
    getPost(){    
      var url = 'https://jsonplaceholder.typicode.com/posts'
      var endpoint = ''
      if (this.UserId.id != undefined && this.UserId.id != ''){ 
        endpoint = '?userId='+this.UserId.id
      }
      this.$http
        .get(url+endpoint)
        .then(function(response) {
          this.posts = response.body
        })
    }
	},

  mounted() {
    this.getPost()
  },
  watch: {
    UserId: function() {
      this.getPost()
    }
  }

}


</script>
<style>
tr:hover {background-color: #f5f5f5;}
</style>