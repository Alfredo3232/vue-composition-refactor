<template>
  <base-container v-if="user">
    <h2>{{ user.fullName }}: Projects</h2>
    <base-search
      v-if="hasProjects"
      :search-term="enteredSearchTerm"
      @search="updateSearch"
    />
    <ul v-if="hasProjects">
      <project-item
        v-for="prj in availableProjects"
        :key="prj.id"
        :title="prj.title"
      />
    </ul>
    <h3 v-else>
      No projects found.
    </h3>
  </base-container>
  <base-container v-else>
    <h3>No user selected.</h3>
  </base-container>
</template>

<script>
import { ref, computed, watch, toRefs } from "vue";
import ProjectItem from "./ProjectItem.vue";

export default {
  components: {
    ProjectItem,
  },
  props: ["user"],
  setup(props) {
    let enteredSearchTerm = ref("");
    let activeSearchTerm = ref("");

    let updateSearch = (val) => {
      enteredSearchTerm.value = val;
    };

    watch(enteredSearchTerm, (val) => {
      setTimeout(() => {
        if (val === enteredSearchTerm.value) {
          activeSearchTerm.value = val;
        }
      }, 300);
    });
    const { user } = toRefs(props);
    watch(user, () => {
      enteredSearchTerm.value = "";
    });

    let availableProjects = computed(() => {
      if (activeSearchTerm.value) {
        return props.user.projects.filter((prj) =>
          prj.title.includes(activeSearchTerm.value)
        );
      }
      return props.user.projects;
    });
    let hasProjects = computed(() => {
      return props.user.projects && availableProjects.value.length > 0;
    });

    return {
      enteredSearchTerm,
      activeSearchTerm,
      updateSearch,
      availableProjects,
      hasProjects
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