<template>
  <base-container>
    <h2>Active Users</h2>
    <base-search
      :search-term="enteredSearchTerm"
      @search="updateSearch"
    />
    <div>
      <button
        :class="{ selected: sorting === 'asc' }"
        @click="sort('asc')"
      >
        Sort Ascending
      </button>
      <button
        :class="{ selected: sorting === 'desc' }"
        @click="sort('desc')"
      >
        Sort Descending
      </button>
    </div>
    <ul>
      <user-item
        v-for="user in displayedUsers"
        :id="user.id"
        :key="user.id"
        :user-name="user.fullName"
        @list-projects="$emit('list-projects', $event)"
      />
    </ul>
  </base-container>
</template>

<script>
import { ref, computed, watch } from "vue";
import UserItem from "./UserItem.vue";

export default {
  components: {
    UserItem,
  },
  props: ["users"],
  setup(props, context) {
    let enteredSearchTerm = ref("");
    let activeSearchTerm = ref("");
    let sorting = ref(null);

    // methods functions
    function updateSearch(val) {
      enteredSearchTerm.value = val;
    }
    function sort(mode) {
      sorting.value = mode;
    }

    // computed functions
    let availableUsers = computed(() => {
      let users = [];
      if (activeSearchTerm.value) {
        users = props.users.filter((usr) =>
          usr.fullName.includes(activeSearchTerm.value)
        );
      } else if (props.users) {
        users = props.users;
      }
      return users;
    });
    let displayedUsers = computed(() => {
      if (!sorting.value) {
        return availableUsers.value;
      }
      return availableUsers.value.slice().sort((u1, u2) => {
        if (sorting.value === "asc" && u1.fullName > u2.fullName) {
          return 1;
        } else if (sorting.value === "asc") {
          return -1;
        } else if (sorting.value === "desc" && u1.fullName > u2.fullName) {
          return -1;
        } else {
          return 1;
        }
      });
    });

    // watch functions
    watch(enteredSearchTerm, function (val) {
      setTimeout(() => {
        if (val === enteredSearchTerm.value) {
          activeSearchTerm.value = val;
        }
      }, 300);
    });

    // return
    return {
      // data
      enteredSearchTerm,
      activeSearchTerm,
      sorting,
      //methods
      updateSearch,
      sort,
      // computed
      availableUsers,
      displayedUsers
    };
  }
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>