<template>
  <main>
    <user-list
      :users="activeUsers"
      @list-projects="selectUser"
    />
    <projects-list :user="selectedUser" />
  </main>
</template>

<script>
import { ref } from "vue";
import USER_DATA from "./dummy-data.js";

import UserList from "./components/users/UserList.vue";
import ProjectsList from "./components/projects/ProjectsList.vue";

export default {
  components: {
    UserList,
    ProjectsList
  },
  setup() {
    // data
    let selectedUser = ref(null);
    const activeUsers = USER_DATA;

    // functions
    function selectUser(uid) {
      selectedUser.value = activeUsers.find((usr) => usr.id === uid);
    }

    // return data and functions
    return {
      selectedUser,
      activeUsers,
      selectUser
    };
  }
};
</script>

<style>
* {
  box-sizing: border-box;
}

html {
  font-family: sans-serif;
}

body {
  margin: 0;
}

main {
  display: flex;
  justify-content: space-around;
}

button {
  font: inherit;
  border: 1px solid #00006b;
  background-color: transparent;
  color: #00006b;
  padding: 0.5rem 1.5rem;
  cursor: pointer;
  margin: 0.5rem 0.5rem 0.5rem 0;
}

button:hover,
button:active {
  background-color: #efefff;
}

button.selected {
  background-color: #00006b;
  color: white;
}
</style>