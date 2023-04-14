<template>
    <li>
        <label v-if="!isEdit">
            <input type="checkbox" :checked="todo.check" @click="change" :id="todo.id"/>
            <span>{{todo.name}}</span>
        </label>
        <input type="text" v-if="isEdit" :value="todo.name" v-on:keyup.enter="edit" v-on:focusout="edit" ref="inputName">
        <button class="btn btn-danger" @click="deleteTodo" :id="todo.id">删除</button>
        <button class="btn btn-edit" @click="handleEdit" :id="todo.id" v-show="!isEdit">编辑</button>
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
            },
            edit(e){
                if(e.target.value.trim()){
                    let obj = {id: this.todo.id,name: e.target.value.trim(),check: this.todo.check}
                    this.$bus.$emit("edit",obj)
                    this.isEdit = false;
                }
            },
            handleEdit(){
                this.isEdit = true;
                this.$nextTick(function () {
                    this.$refs.inputName.focus();
                })
            }
        },
        data(){
            return {
                isEdit: false,
                isFocus: true,
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