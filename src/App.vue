<template>
    <div id="root">
        <div class="todo-container">
            <div class="todo-wrap">
                <Add :addItem="addItem"></Add>
                <List :things="things" :changeCheck="changeCheck" :deleteOne="deleteOne"></List>
                <Footer :completedNums="completedNums"
                        :todoNums="todoNums"
                        :chooseAll="chooseAll"
                        :clearDone="clearDone"
                        :isAll="isAll"/>
            </div>
        </div>
    </div>
</template>

<script>
import Add from "./components/Add";
import List from "./components/List";
import Footer from "./components/Footer";

export default {
  name: 'App',
  components: {
    Footer,
    List,
    Add
  },
  data(){
    return {
        things: JSON.parse(window.localStorage.getItem("things")) || []
    }
  },
    computed:{
        completedNums(){
          return this.things.filter((p)=>{
              return p.check
          }).length
      },
        todoNums(){
            return this.things.length
        }
    },
    methods: {
        addItem(a){
          this.things.unshift(a)
      },
        changeCheck(id){
           for(let i=0;i<this.things.length;i++){
               if(this.things[i].id === id){
                   this.things[i].check = !this.things[i].check;
               }
           }
        },
        deleteOne(id){
           let arr = this.things.filter((p)=>{
               return p.id !== id;
           })
            this.things = arr;
        },
        chooseAll(checked){
            for(let i=0;i<this.things.length;i++){
                if(checked){
                    this.things[i].check = true;
                }
                else {
                    this.things[i].check = false;
                }

            }
        },
        clearDone(){
            this.things = this.things.filter((p)=>{
                return !p.check
            })
        },
        isAll(){
            return this.completedNums===this.todoNums
        },
        edit(newObj){
            for(let i = 0;i<this.things.length;i++){
                if(this.things[i].id === newObj.id){
                    this.things.splice(i,1,newObj)
                }
            }
        }

    },
    watch: {
      things: {
          deep: true,
          handler(value){
              window.localStorage.setItem("things",JSON.stringify(value))
          }
      }
    },
    mounted() {
      this.$bus.$on("edit",this.edit)
    }

}
</script>

<style>
    /*base*/
    body {
        background: #fff;
    }

    .btn {
        display: inline-block;
        padding: 4px 12px;
        margin-bottom: 0;
        font-size: 14px;
        line-height: 20px;
        text-align: center;
        vertical-align: middle;
        cursor: pointer;
        box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
        border-radius: 4px;
    }

    .btn-danger {
        color: #fff;
        background-color: #da4f49;
        border: 1px solid #bd362f;
    }
    .btn-edit{
        color: #ffffff;
        background-color: skyblue;
        border: solid #5ea9eb 1px;
        margin-right: 5px;
    }

    .btn-danger:hover {
        color: #fff;
        background-color: #bd362f;
    }

    .btn:focus {
        outline: none;
    }

    .todo-container {
        width: 600px;
        margin: 0 auto;
    }
    .todo-container .todo-wrap {
        padding: 10px;
        border: 1px solid #ddd;
        border-radius: 5px;
    }







</style>
