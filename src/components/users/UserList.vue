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
import UserItem from "./UserItem.vue";

export default {
  components: {
    UserItem,
  },
  props: ["users"],
  data() {
    return {
      enteredSearchTerm: "",
      activeSearchTerm: "",
      sorting: null
    };
  },
  computed: {
    availableUsers() {
      let users = [];
      if (this.activeSearchTerm) {
        users = this.users.filter((usr) =>
          usr.fullName.includes(this.activeSearchTerm)
        );
      } else if (this.users) {
        users = this.users;
      }
      return users;
    },
    displayedUsers() {
      if (!this.sorting) {
        return this.availableUsers;
      }
      return this.availableUsers.slice().sort((u1, u2) => {
        if (this.sorting === "asc" && u1.fullName > u2.fullName) {
          return 1;
        } else if (this.sorting === "asc") {
          return -1;
        } else if (this.sorting === "desc" && u1.fullName > u2.fullName) {
          return -1;
        } else {
          return 1;
        }
      });
    }
  },
  watch: {
    enteredSearchTerm(val) {
      setTimeout(() => {
        if (val === this.enteredSearchTerm) {
          this.activeSearchTerm = val;
        }
      }, 300);
    }
  },
  methods: {
    updateSearch(val) {
      this.enteredSearchTerm = val;
    },
    sort(mode) {
      this.sorting = mode;
    }
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