

<template>
<!-- При написании кода мне пришлось гуглить много новой информации.  -->
<!-- Некоторые из концептов с которыми мне пришлось столкнуться кажутся мне сложными. -->
<!-- Если честно, часть написанного я не понимаю, однако это было интересно и я узнал много нового. -->
<!-- Не считаю себя сильным front-end разработчиком, но в данный момент я хочу попробовать себя в роли стажера. -->


<div class="userWrapper" >
    <img :src="userInfo.avatar">
    <div class="wrapOne">
        <h3>First Name: </h3>
        <p v-if="!editing">{{userInfo.firstName}}</p>
        <input v-if="editing" type="text" v-model="currentUser.firstName">
        <button class="btnOpen" @click="open = !open">{{open ? "close" : "expand"}}</button>
    </div>

    <div class="dialogue" v-if="open">
        <h3>Last Name: </h3>
        <p v-if="!editing">{{userInfo.lastName}}</p>
        <input v-if="editing" type="text" v-model="currentUser.lastName">
        <h3>E-mail: </h3>
        <p v-if="!editing">{{userInfo.email}}</p>
        <input v-if="editing" type="text" v-model="currentUser.email">
        <button class="btnDelete" @click="deleteUser(index)">Delete</button>
        <button class="btnEdit" @click="editUser()">{{editing ? "save" : "edit"}}</button>
    </div>
</div>
</template>

<script lang='ts'>
import { defineComponent, ref } from "vue";

export default defineComponent({
    name: 'User',
    emits: ["delete", "edit"],
    props: {
        userInfo: Object,
        index: Number
    },
    setup(props, {emit}){
        const open = ref<boolean>(false)
        const editing = ref<boolean>(false)
        const currentUser = ref<any>(props.userInfo)
        currentUser.value.index = props.index
        function deleteUser(index: Number){
            emit("delete", index)
        }
        function editUser(){
            if(editing.value){
                emit("edit", currentUser.value)
            }
            editing.value = !editing.value
        }
        return{
            open,
            deleteUser,
            currentUser,
            editing,
            editUser
        }
    }

})
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Caveat:wght@400;500;600;700&display=swap');

@import url('https://fonts.googleapis.com/css2?family=Caveat:wght@400;500;600;700&family=Noto+Sans&family=Playfair+Display&display=swap');

.userWrapper{
    /* background-color: #43ADEB; */
    background-color: #FFFFFF;
    height: 62%;
    width: 220px;
    display: flex;
    flex-direction: column;
    padding: 15px;
    border-radius: 20px;
    box-sizing: border-box;
    
    filter: drop-shadow(0 0 0.75rem rgba(121, 121, 121, 0.562));
}
img{
    width: 100px;
    height: 100px;
    border-radius: 50px
}
.btnDelete{
    position: absolute;
    right: 15px;
    height: 40px;
    width: 60px;
    bottom: -20px;
    /* font-family: 'Caveat', cursive; */
    background-color: #F2F3F5;
    color: black;
    border: 2px solid #535252
}
.btnOpen{
    position: absolute;
    height: 30px;
    padding: 0px 10px;
    right: 20px;
    top: 60px;
    border-radius: 5px;
    border: 2px solid #e7e7e7;
    color: #858484;
}
.btnEdit{
    position: absolute;
    bottom: 15px;
    left: 15px;
    padding: 5px;
    border-radius: 30%;
    border: 1.3px solid #a7a6a6;
}
h3{
    /* font-family: 'Playfair Display', serif; */
    font-size: 18px;
}
p{
    font-family: 'Playfair Display', serif;
    font-size: 14px;
    font-weight: 600;
}

</style>
