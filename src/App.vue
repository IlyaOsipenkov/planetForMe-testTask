



<template>
  <div v-if="downloaded" class="main">
    <UserCard :key="User.id" v-for="User in users" :userInfo="User"/>
  </div>
</template>



<script lang="ts">
import {computed, defineComponent, ref} from 'vue'
import axios from 'axios'
import { User } from './models'
import UserCard from './components/User.vue'
export default defineComponent({
  components: {
    UserCard
  },
  setup() {
    const downloaded = ref<boolean>(false)
    const storage = {
      save: function (data: User[]) {
        localStorage.setItem('key', JSON.stringify(data))
      },
      get: function(){
        let data: string | User[] | null = localStorage.getItem('key')
        if(data != null){
          return JSON.parse(data)
        } return null
      }
    }
    function getItems() {
      let data: User[] | [] = []
      axios.get('https://reqres.in/api/users?page=2').then(function (response) {
        for (let i of response.data.data) {
          let user = new User()
          user.avatar = i.avatar
          user.email = i.email
          user.firstName = i.first_name
          user.id = i.id
          user.lastName = i.last_name
          data = [...data, user]
        }
          storage.save(data)
          downloaded.value = true
      })
    }
    getItems()
    const users = computed(()=>{
      console.log(storage.get())
      return storage.get()
    })
    return {
      users, downloaded
    }
  } 
})
</script>



<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
.main{
  height: 100vh;
  width: 100vw;
  background-color: antiquewhite;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(2, 1fr);
  justify-items: center;
  align-items: center;
}
</style>
