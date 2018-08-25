<!--双向数据绑定实现todolist-->
<template>
  <div id="app">
    <img src="./assets/logo.png">
    <h1>{{ msg }}</h1>

    <input type="text" v-model="todo"/>

    <button @click="doAdd">+增加</button>

    <br/>
    <hr>
    <h2>进行中</h2>
    <ul>
      <li v-for="(item, key) in list" v-if="!item.checked">
        <input type="checkbox" v-model="item.checked" @change="saveList"/>
        {{key}} ---{{item.title}}  &nbsp;&nbsp;&nbsp;&nbsp;
        <button @click="removeData(key)">删除事件</button>
      </li>
    </ul>

    <br/>
    <hr>
    <h2>已完成</h2>
    <ul class="finish">
      <li v-for="(item, key) in list" v-if="item.checked">
        <input type="checkbox" v-model="item.checked" @change="saveList"/>
        {{key}} ---{{item.title}}  &nbsp;&nbsp;&nbsp;&nbsp;
        <button @click="removeData(key)">删除事件</button>
      </li>
    </ul>



  </div>
</template>

<script>

import storage from './model/storage'

export default {
  name: 'app',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      list: []
    }
  },
  methods: {
    doAdd(){
      //1.获取文本框的数据    2.吧文本框数据push到list中
      this.list.push({
        title: this.todo,
        checked: false
      });

      this.todo = '';

      // 每次增加时使用localStorage存储list

      //localStorage.setItem('list', JSON.stringify(this.list))

      // 使用封装好的storage
      storage.set('list', this.list)

    },
    removeData(key){
      /*splice用于删除或替换指定的值
      * 原生js的方法*/
      this.list.splice(key, 1);

      // 每次删除之后也使用localStorage再次将更新后的list进行保存
      //localStorage.setItem('list', JSON.stringify(this.list))

      // 使用封装好的storage
      storage.set('list', this.list)
    },
    saveList(){
      //localStorage.setItem('list',JSON.stringify(this.list));

      // 使用封装好的storage
      storage.set('list', this.list);
    }

  },
  mounted(){
    /*生命周期函数
    * vue页面刷新就会触发方法
    * 该生命周期函数是放在methods之外的*/
    //var list = JSON.parse(localStorage.getItem('list'));

    // 使用封装好的storage
    var list = storage.get('list')
    if(list){
      this.list = list;
    }
  }

}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}



a {
  color: #42b983;
}

  .finish {
    li{
      background-color: #eee;
    }
  }
</style>
