<template>
  <div class='input-wrapper'>
    <div class='inputing'>
      <input type="text" v-model='newItem' placeholder="請輸入事項">
    </div>
    <div class='create-btn' @click='createTodo'><i class="fas fa-plus-circle fa-2x"></i></div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      newItem:''
    }
  },
  methods: {
    createTodo(){

      let newText = this.newItem.trim();

      if(newText == false){
        console.log(newText);
        return;
      }


      this.$emit('create', this.newItem);
      this.newItem= '';
    },
  },

  mounted () {
    browser.runtime.sendMessage({})
  }
}
</script>

<style scoped>
.input-wrapper{
  box-sizing: border-box;
  width: 100%;
  padding: 0.25rem;
  border-bottom: 2px solid orange;
  display: flex;
  justify-content: space-between;
}

.inputing{
  width: 85%;
}

.create-btn{
  color: white;
  width: 10%;
  background: transparent;
  cursor: pointer;
}

.create-btn:hover { 
  color: orange;
}
/* 
.create-btn i{
  color:white;
} */

.input-wrapper input{
  width: 100%;
  padding:5px;
  border: none;
  border-radius: 3px;
  border: none;
}
</style>
