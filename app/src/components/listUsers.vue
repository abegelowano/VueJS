<template>
	<div id="list-users">
    <h4><center><strong>Users</strong></center></h4>
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
      <a class="button is-primary" v-on:click="createUser()">
        <strong>Create User </strong>
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

          <h6 style="padding-top: 10px;">Name</h6>
          <input type="text" placeholder="Name" class="input" v-model="createuser.name">
    
          <h6 style="padding-top: 10px;">Username</h6>
          <input type="text" placeholder="Username" class="input" v-model="createuser.username">

          <h6 style="padding-top: 10px;">Email</h6>
          <input type="text" placeholder="Email" class="input" v-model="createuser.email">

          <h6 style="padding-top: 10px;">City</h6>
          <input type="text" placeholder="City" class="input"  v-model="createuser.address.city">

          <h6 style="padding-top: 10px;">Street</h6>
          <input type="text" placeholder="Street" class="input" v-model="createuser.address.street">
        </section>
        <footer class="modal-card-foot">
          <button class="button is-success" v-on:click ="post_patch(createuser)">Save</button>
          <button class="button" v-on:click="cancel()">Cancel</button>
        </footer>
      </div>
    </div>



    <!-- list -->
    <table>
      <thead>
        <th width="15%">Name</th>
        <th width="5%">User Name</th>
        <th width="6%">Email</th>
        <th width="10%">City</th>
        <th width="15%">Street</th>
        <th width="15%" style="text-align:center">Action</th>
      </thead>
      <tbody>
        <tr v-for="(user,index) in users" v-bind:key="user.id" >
          <td width="15%">{{ user.name}}</td>
          <td width="5%">{{user.username}}</td>
          <td width="6%">{{user.email}}</td>
          <td width="10%">{{user.address.city}}</td>
          <td width="15%">{{user.address.street}}</td>
          <td width="15%">    
            <div style="text-align:center">
              <a class="button is-primary" v-on:click="editUser(user);showCreateModal=true">
                <strong>Edit</strong>
              </a>&nbsp;&nbsp;
              <a class="button is-primary" v-on:click="deleteuser(user,index)">
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
	data(){
		return {
      modal: {
        title: "",
        msg: "",
        action: ""
      },
      showCreateModal: false,
      showOkayModal: false,
      users: [],
      createuser: {
        'name': '',
        'username': '',
        'email':'',
        'address': {
          'city': '',
          'street': ''
        }
      }
		}
	},
	methods: {
    deleteuser(user,index){
      this.$http
      .delete('https://jsonplaceholder.typicode.com/users/'+user.id)
      .then(function() {
        this.showOkayModal= true;
        this.modal.msg = "Successfully Deleted!"
        this.users.splice(index, 1);
      })
    },
    post_patch(user){
      if (this.modal.action==="POST"){
        this.$http
        .post('https://jsonplaceholder.typicode.com/users',this.createuser)
        .then(function(response) {
          this.users.push(response.body);
          this.showCreateModal = false;
          this.showOkayModal= true;
          this.modal.msg = "Successfully Saved!"
          this.createuser = {'name': '','username': '','email':'','address': {'city': '','street': ''}}

        });
      } else if (this.modal.action==="PATCH"){
        this.$http
        .patch('https://jsonplaceholder.typicode.com/users/'+user.id,user)
        .then(function() {
          // this.users.push(response.body);
          // console.log(response)
          this.showCreateModal = false;
          this.showOkayModal= true;
          this.modal.msg = "Successfully Updated!"
          this.createuser = {'name': '','username': '','email':'','address': {'city': '','street': ''}}

        });
      }

    },
    createUser(){
      this.showCreateModal = true
      this.modal.title = 'Create New User'
      this.modal.action = "POST"
    },
    editUser(user){
      this.showCreateModal = true
      this.createuser = user
      this.modal.title = 'Edit User'
      this.modal.action = "PATCH"
    },
    cancel(){
      this.showCreateModal=false
      this.createuser = {'name': '','username': '','email':'','address': {'city': '','street': ''}}
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
<style>
tr:hover {background-color: #f5f5f5;}
</style>