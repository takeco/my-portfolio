<template>
    <div class="body">
      <header>
        <h1>Todolist</h1>
          <nav>
            <button @click="purge">完了を削除</button>
            <p>未完了({{remaining.length}})</p>
          </nav>
      </header>
      <form @submit.prevent="addTodo">
        <input v-model="newTodo" type="text" placeholder="  入力       １５文字まで"  maxlength='20'>
        <input type="submit" value="追加">
      </form>
      <section>
        <ul v-if="todos.length">
          <li v-for="(todo, index) in todos" :key="todo">
            <input type="checkbox" v-model="todo.isDone">
            <span :class="{done: todo.isDone}">{{todo.title}}</span>
            <img @click="deleteTodo(index)" src="../../public/img/dustBox.svg" alt="" id="dustBox">
            <form @submit.prevent>
              <input v-show="todo.editor" type="text" v-model="todo.title">
              <button @click="todo.editor =!todo.editor">
                <span v-show="!todo.editor">編集</span><span v-show="todo.editor">完了</span>
              </button>
            </form>
            <p>memo</p>
            <textarea type="text" placeholder="メモ" v-model="todo.memo"></textarea>
          </li>
        </ul>
        <ul v-else>
          <li>買うものを入力</li>
        </ul>
      </section>
    </div>
</template>

<script>
export default {
  data(){
    return{
      newTodo:'',
      memo:'',
      todos:[]
        };
  },
  watch:{
    todos:{
      handler(){
        localStorage.setItem('todos',JSON.stringify(this.todos));
      },
      deep:true
    }
  },
  mounted(){
    this.todos = JSON.parse(localStorage.getItem('todos')) || [];
  },
  methods: {
    addTodo(){
      let item = {
        title:this.newTodo,
        isDone:false,
        editor:false,
        memo:this.memo
      };
      this.todos.push(item);
      this.newTodo = '';
    },
    deleteTodo(index){
      this.todos.splice(index, 1);
    },
    purge(){
      if(confirm('完了したものを削除します')){
      this.todos  = this.remaining;
        }
    },
  },
  computed: {
    remaining(){
      return this.todos.filter(function(todo){
        return !todo.isDone;
      });
    }
  },
  };
</script>

<style>
/* width 400px未満 */

.body{
  margin: 0;
  display:flex;
  flex-flow: column nowrap;
  justify-content: center;
  padding: 20px;
}
header{
  background-color: rgba(255, 255, 255, 0.8);
}
header>h1{
  text-align: center;
}
header>nav{
  text-align: right;
}
.body>form{
  text-align: center;
  padding: 20px 0;
}
.body>form>input:first-of-type{
  width: 60%;
  height: 20px;
}
section{
  background-color: rgba(255, 255, 255, 0.8);
}

#dustBox{
  height: 20px;
  cursor: pointer;
}
section>ul{
    list-style: none;
    max-width: 600px;
  word-wrap: break-word;
  width: 80%;
  margin: 0 auto;
}
section>ul>li{
  border-bottom: solid 1px #bbb;
  padding:10px 0;
  display: grid;
  grid-template:  "checkbox todoName dustBox" 30px
                  "edit     edit     edit   " 30px
                  "    .    memo        .   " auto
                  / 50px 1fr 50px;
}
section>ul>li>input:first-of-type{
  grid-area: checkbox;
  height: 40px;
  
}
section>ul>li>span:first-of-type{
  padding-left: 5px;
  grid-area: todoName;
  word-wrap: anywhere;
  font-weight: 600;
}
section>ul>li>img:first-of-type{
  grid-area: dustBox;
}
  section>ul>li>form{
  grid-area: edit;
  text-align: right;
}
section>ul>li>textarea{
  grid-area: memo;
  overflow-wrap: anywhere;

}
  section>ul>li>form>input{
    line-height: 20px;
}



.done{
  text-decoration: line-through;
  color: #bbb;
}


/* width 800px未満 */
@media (min-width: 400px){
  header{
  position: relative;
  background-color: rgba(255, 255, 255, 0.8);
  }
  header>nav{
    position: absolute;
    text-align: right;
    right:10px;
    top: 10px;
  }

}

/* width 800px以上 */
@media (min-width: 800px){
  .body{
    width: 800px;
    margin:  auto;
  }
}

</style>