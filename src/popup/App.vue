<template>
  <div class='wrapper'>
    <h1 class='title'>{{ extentionName }}</h1>
    <hello-world v-on:create="createHandler"></hello-world>

    <ul class='list-wrapper'>

        <li v-for='(item, index) in todoList' :key='item.id' class='each-todo'>
        
        <template v-if='editingId==item.id'>
          <input type="text" class='editInput' value='editingContent' v-model='editingContent'>
          <button @click='confirmEdit(index)' class='edit-btn'>確認</button>
          <button @click='cancleEdit' class='edit-btn'>取消</button>
        </template>

      
        <template v-else>
          <input :id='item.id' class='check' type="checkbox" v-model='item.completed' @change="checkEvent">

          <label :for='item.id' class='todo-item' :class={finish:item.completed}>{{ item.todo }}</label>

          <span class='icon-edit' @click='editingHandler(item)'>
            <i class="far fa-edit"></i>
          </span>

          <span class='icon-remove' @click='removeItem(index)'>
            <i class="fas fa-trash"></i>
          </span>
        </template>
        
        </li>
    </ul>
  </div>
</template>

<script>
import HelloWorld from '@/components/HelloWorld.vue'


// let todos = [
//   {todo:'running', completed: true, id:'001'},
//   {todo:'writing', completed: true, id:'002'},
//   {todo:'sleeping', completed: true, id:'003'},
  
// ];

// chrome.storage.sync.set({todoList: todos}, function() {
//   console.log('set OK')
// });

// chrome.storage.sync.get(['todoList'], function(result) {
//   let data =  JSON.parse(result['todoList']);
//   console.log('myData', data)
// });

export default {
  name: 'App',
  components: { HelloWorld },
  data() {
    return {
      extentionName:'代辦清單',
      user:'jared',
      todoList: [],

      editingId:'',
      editingContent:'',
    }
  },

  methods: {
    createHandler(value){

      let id = Date.now();
      let newItem = { todo: value, completed: false, id};
      this.todoList.push(newItem);
      this.saveData();

    },
    checkEvent(){
        // this.todoList[index].completed = !this.todoList[index].completed
        this.saveData();
    },
    removeItem(index){
      this.todoList.splice(index, 1);
      this.saveData();
    },
    editingHandler(item){
      this.editingId = item.id;
      this.editingContent = item.todo;
    },
    confirmEdit(index){
      this.todoList[index].todo = this.editingContent;
      this.editingContent='';
      this.editingId='';
      this.saveData();
    },
    cancleEdit(){
      this.editingContent='';
      this.editingId='';
    },
    saveData(){

      let todos = this.todoList;
      chrome.storage.sync.set({todoList: todos}, function() {
        console.log('set OK')
      });

    },

    getData(){

      //需要使用箭頭，才可以拿到 Vue
      chrome.storage.sync.get(['todoList'], (result) =>{
        let data = result['todoList'] || [{todo:'跑步運動3k', completed: true, id:'003'}];
        this.todoList = data;
    }); 

    },
  },


  created() {
    this.getData();

    // chrome.storage.sync.get(item=>{console.log(item)})
    // chrome.storage.local.get(function(result){console.log(result)})
  },


}
</script>

<style src="./style.css"></style>
