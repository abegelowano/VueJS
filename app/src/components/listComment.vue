<template>
  <div id="list-comment">
    <h4><center>Comments</center></h4>
    <h4>Title: {{PostId.title}}</h4>

    <!-- list -->
    <table >
      <thead>
        <th width="5%">Post Id</th>
        <th width="15%">Name</th>
        <th width="15%">Email</th>
        <th width="20%">Comments</th>
      </thead>
      <tbody>
        <tr v-for="comment in comments" v-bind:key="comment.id" >
          <td width="5%">{{comment.postId}}</td>
          <td width="15%">{{comment.name}}</td>
          <td width="15%">{{comment.email}}</td>
          <td width="20%">{{comment.body}}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
  
<script>

export default {
  props:[
    'PostId'
  ],
  data(){
    return {
      comments: [],
    }
  },
  methods: {
    getComments(){    
      var url = 'https://jsonplaceholder.typicode.com/comments'
      var endpoint = ''
      if (this.PostId.id != undefined && this.PostId.id != ''){ 
        endpoint = '?postId='+this.PostId.id
      }
      this.$http
        .get(url+endpoint)
        .then(function(response) {
          this.comments = response.body
        })
    }
  },

  mounted() {
    this.getComments()
  },
  watch: {
    PostId: function() {
      this.getComments()
    }
  }

}


</script>
<style>
tr:hover {background-color: #f5f5f5;}
</style>