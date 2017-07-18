<template>
  <div id="app">
    <h1 v-text="title"></h1>
    <input class="vinput" v-model="newitem" v-on:keyup.enter="additem">
    <ul>
      <li>{{newitem}}</li>
      <li v-for="item in items" @mouseenter='seedelete(item)' @mouseleave='hidedelete(item)'>
        <input type='checkbox' v-on:click="togglefinished(item)" v-bind:checked='item.isFinished'>
        <p   v-bind:class='{finished:item.isFinished}'>{{items.indexOf(item)+1}}.{{item.object}}</p>
        <p class="vfinished" v-if='item.isFinished'>finished</p>
        <p class="vdeleted" v-if='item.showdelete' v-on:click="deleteitem(item)">delete</p>
      </li>
    </ul>
  </div>
</template>

<script>
import Store from './store'
export default {
  data () {
    return {
      title:'This is a todo list',
      items: Store.fetch (),
      newitem:''
    }
  },
  watch: {
    items: {
      handler: function(items) {
        Store.save(items);
      },
      deep: true
    }
  },
  methods:{
    togglefinished:function(item){
      item.isFinished=!item.isFinished;
    },
    additem:function(){
      this.items.push({
        object:this.newitem,
        isFinished:false,
        showdelete:false

      })
      this.newitem='';
      Store.save(this.newitem);
    },
    seedelete (item) {
      item.showdelete=true;
    },
    hidedelete (item) {
      item.showdelete=false;
    },
    deleteitem (item) {
      var index = this.items.indexOf(item);
     this.items.splice(index, 1)
    }
  }
}
</script>

<style>
*{margin:0;padding:0;}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align:left;
  color: #2c3e50;
  margin: 60px auto;
  width: 400px;
}
h1{
  margin-bottom: 20px;
}
.vinput{
  width: 400px;
  height: 40px;
  border-radius: 5px;
  font-size: 20px;
  padding:0 10px;
  border:none;
  outline: none;
  /*清除移动端默认的表单样式*/
  -webkit-appearance:none;
  border:1px solid black;

}
.finished{
  text-decoration: line-through;
}
li{overflow: hidden;height: 50px;line-height: 50px;}
li p{
  display: inline;
  font-size: 20px;
}
li input{
  margin:auto 15px auto 0;
}
.vfinished,.vdeleted{
  color:#fff;
  background-color:green;
  height:20px;
  font-size: 10px;
  padding: 2px;
  border-radius: 3px;
}
.vdeleted{
  background-color:red;
  cursor:pointer;
}
</style>
