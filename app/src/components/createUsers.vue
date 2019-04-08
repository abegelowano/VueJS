<template>
	<div id="create-users">
    <div class="field is-grouped is-grouped-multiline"> 
      <a class="button is-primary" v-on:click="showCreateModal=true">
        <strong>Create User </strong>
      </a>
    </div>
    <div class="modal is-active" v-show="showCreateModal">
      <div class="modal-background"></div>
      <div class="modal-content">
        
        <header class="modal-card-head">
          <p class="modal-card-title">Create New User</p>
          <button class="delete" aria-label="close" @click="showCreateModal=false"></button>
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

          <div class="field is-grouped is-grouped-multiline"> 
            <a class="button is-primary" v-on:click.prevent ="post">
              <strong>Submit</strong>
            </a>
          </div>
          
        </section>
      </div>
    </div>
    
	</div>
</template>
	
<script>

export default {
	data(){
		return {
      showCreateModal:false,
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
    post(){
      this.$http
      .post('https://jsonplaceholder.typicode.com/users',this.createuser)
      .then(function(response) {
        
      })
    }

	},
}


</script>
<style>

</style>