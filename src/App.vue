



<template>
  <div v-if="downloaded" class="main">
    <UserCard
      @delete="deleteUser(index)"
      @edit="editUser"
      :key="User.id"
      v-for="(User, index) in users"
      :userInfo="User"
      :index="index"
    />
  </div>
  <button class="buttonRefresh" @click="getItems()">Get from API</button>
</template>



<script lang="ts">
import { computed, defineComponent, ref } from "vue";
import axios from "axios";
import { User } from "./models";
import UserCard from "./components/User.vue";
import { error } from "console";
export default defineComponent({
  components: {
    UserCard,
  },
  setup() {
    const downloaded = ref<boolean>(false);
    function editUser(user: User):void{
      if(user.index){
      let allUsers: User[] = users.value
      allUsers[user.index] = user
      storage.save(allUsers)
      }else{
        throw new Error("user is not defined")
      }
    }
    const storage = {
      save: function (data: User[]) {
        localStorage.setItem("key", JSON.stringify(data));
      },
      get: function () {
        let data: string | User[] | null = localStorage.getItem("key");
        if (data != null) {
          return JSON.parse(data);
        }
        return null;
      },
    };
    function getItems() {
      let data: User[] | [] = [];
      axios.get("https://reqres.in/api/users?page=2").then(function (response) {
        for (let i of response.data.data) {
          let user = new User();
          user.avatar = i.avatar;
          user.email = i.email;
          user.firstName = i.first_name;
          user.id = i.id;
          user.lastName = i.last_name;
          data = [...data, user];
        }
        storage.save(data);
        downloaded.value = true;
        users.value = storage.get()
      }).catch(function(error){
        throw new Error(error)
      });
    }
    getItems();
    const users = ref<User[]>(storage.get());
    function deleteUser(index: number) {
      let tempUsers = users.value;
      tempUsers.splice(index, 1);
      storage.save(tempUsers);
      users.value = storage.get();
    }
    return {
      users,
      downloaded,
      deleteUser,
      getItems,
      editUser
    };
  },
});
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
.main {
  height: 100vh;
  width: 100vw;
  background-color: #E5E5E5;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(2, 1fr);
  justify-items: center;
  align-items: center;
  position: relative;
  gap: 20px;
}
.buttonRefresh{
  background-color: #F2F3F5;
  position: absolute;
  width: 100px;
  height: 60px;
  border-radius: 120px;
  top: 15px;
  left: calc(50vw - 100px / 2);
}
</style>
