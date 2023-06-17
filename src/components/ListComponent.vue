<template>
<div>
        <h1>{{ txtmsg }}</h1>
        <button type="button" 
                class="btn btn-primary" 
                data-bs-toggle="modal" 
                data-bs-target="#exampleModal">
  Create A Topic
</button>
<ul class="list-group">
    <li class="list-group-item d-flex justify-content-between align-items-center" 
        v-for="item in data" 
        :key="item.guid"
        >
      {{ item.name }}
      <button class="btn btn-danger"
             @click="deleteItem(item.guid)"
             >
             delete
      </button>
    </li>
  </ul>
 
 

  <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">Create Topic</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <div class="modal-body">
        <form v-on:submit.prevent>
  <div class="form-group">
    <label for="topicTitle">Topic Title</label>
    <input type="text" class="form-control" id="topicTitle" aria-describedby="topicTitle" placeholder="Topic Title">
    
  </div>
  <button type="button" data-bs-dismiss="modal" class="btn btn-primary" @click="create">add Topic</button>
</form>
      </div>
      <!-- <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
        <button type="button" class="btn btn-primary">Save changes</button>
      </div> -->
    </div>
  </div>
</div>
</div>
</template>
    
    <script>
    import axios from 'axios';
    export default {
      name: 'ListComponent',
      props: {
        txtmsg:{
            type:String,
            required: true,
        },
      },
      data(){
        return{
            data:[],
            activeItemKey: null,

        }
      },
      mounted(){
           this.fetchdata();
      },
      methods:{
        fetchdata(){
            axios.get('https://atillc.blob.core.windows.net/data-collector/icode/test-data/topics.json')
                 .then(response =>{
                        this.data =response.data.topics.reverse();
                 }).catch(error =>{
                    console.error(error);
                 });
        },
        select(id){
            alert('clicked item with GUId:'+id);
        },
        deleteItem(guid){
          const idToDelete = guid;
           //find index
           const indexToRemove=this.data.findIndex((obj)=> obj.guid ===idToDelete);
           if(indexToRemove !== -1){
            this.data.splice(indexToRemove, 1);
            alert('topic deleted');
            console.log("Object removed successfully:");
            console.log(this.data.length);
           }else{
            console.log("Object with ID not found.");
           }

        },
        create(){
            const title = document.getElementById('topicTitle').value;
            const topic = 
                {name:title,
                  guid:"this should be auto generated",
                };
            
            this.data= [topic,...this.data]; 
           
          alert('new Topic was added successfully');
           console.log('after adding datais now ='+this.data.length);
        }
      }



    }
    </script>
    <style>
  .active{
    background-color: red;
    color:#fff;
  }
</style>