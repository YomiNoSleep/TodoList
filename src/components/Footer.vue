<template>
    <div class="todo-footer" v-show="todoNums">
        <label>
            <input type="checkbox" @click="choose" :checked="isAll()"/>
        </label>
        <span>
          <span>已完成{{completedNums}}</span> / 全部{{todoNums}}
        </span>
        <button class="btn btn-danger" @click="clear">清除已完成任务</button>
    </div>
</template>

<script>
    export default {
        name: "Footer",
        props:["completedNums","todoNums","chooseAll","clearDone","isAll"],
        methods: {
            choose(e){
                this.chooseAll(e.target.checked);
            },
            clear(){
                if(confirm("确定要清除已完成吗")){
                    this.clearDone();
                }
            }
        }
    }
</script>

<style scoped>
    /*footer*/
    .todo-footer {
        height: 40px;
        line-height: 40px;
        padding-left: 6px;
        margin-top: 5px;
    }

    .todo-footer label {
        display: inline-block;
        margin-right: 20px;
        cursor: pointer;
    }

    .todo-footer label input {
        position: relative;
        top: -1px;
        vertical-align: middle;
        margin-right: 5px;
    }

    .todo-footer button {
        float: right;
        margin-top: 5px;
    }
</style>