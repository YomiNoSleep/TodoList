<template>
    <li>
        <label>
            <input type="checkbox" :checked="todo.check" @click="change" :id="todo.id"/>
            <span>{{todo.name}}</span>
        </label>
        <button class="btn btn-danger" @click="deleteTodo" :id="todo.id">删除</button>
    </li>
</template>

<script>
    export default {
        name: "Item",
        props:["todo","changeCheck","deleteOne"],
        methods:{
            change(e){
                this.changeCheck(e.target.id)
            },
            deleteTodo(e){
                if(confirm("确定要删除吗")){
                    this.deleteOne(e.target.id)
                }
            }
        }
    }
</script>

<style scoped>
    /*item*/
    li {
        list-style: none;
        height: 36px;
        line-height: 36px;
        padding: 0 5px;
        border-bottom: 1px solid #ddd;
    }


    li label {
        float: left;
        cursor: pointer;
    }

    li label li input {
        vertical-align: middle;
        margin-right: 6px;
        position: relative;
        top: -1px;
    }

    li button {
        float: right;
        display: none;
        margin-top: 3px;
    }

    li:before {
        content: initial;
    }

    li:last-child {
        border-bottom: none;
    }
    li:hover{
        background-color: #ddd;
    }
    li:hover button{
        display: block;
    }

</style>