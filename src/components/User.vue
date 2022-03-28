

<template>
<div class="userWrapper" @click="open = !open">
    <img :src="userInfo.avatar">
    <h3>First Name: </h3>
    <p>{{userInfo.firstName}}</p>
    <div class="dialogue" v-if="open">
        <h3>Last Name: </h3>
        <p>{{userInfo.lastName}}</p>
        <h3>E-mail: </h3>
        <p>{{userInfo.email}}</p>
    </div>
</div>
</template>

<script lang='ts'>
import { userInfo } from "os";
import { defineComponent, ref } from "vue";

export default defineComponent({
    name: 'User',
    props: {
        userInfo: Object
    },
    setup(props){
        const open = ref<boolean>(false)
        console.log(props.userInfo)
        return{
            open
        }
    }

})
</script>

<style scoped>
</style>
