

<template>
<div class="userWrapper" @click="open = !open">
    <img :src="userInfo.avatar">
    <div class="wrapOne">
        <h3>First Name: </h3>
        <p>{{userInfo.firstName}}</p>
    </div>

    <div class="dialogue" v-if="open">
        <h3>Last Name: </h3>
        <p>{{userInfo.lastName}}</p>
        <h3>E-mail: </h3>
        <p>{{userInfo.email}}</p>
        <button @click="deleteUser(index)">Delete</button>
    </div>
</div>
</template>

<script lang='ts'>
import { userInfo } from "os";
import { defineComponent, ref } from "vue";

export default defineComponent({
    name: 'User',
    emits: ["delete"],
    props: {
        userInfo: Object,
        index: Number
    },
    setup(props, {emit}){
        const open = ref<boolean>(false)
        function deleteUser(index: Number){
            emit("delete", index)
        }
        return{
            open,
            deleteUser
        }
    }

})
</script>

<style scoped>
.userWrapper{
    background-color: azure;
    height: 55%;
    width: 60%;
    display: flex;
    flex-direction: column;
    padding: 5px;
}
img{
    width: 100px;
    height: auto;
}
</style>
