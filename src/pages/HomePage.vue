<template>
  <div class="home-page">
    <!-- Full width, align left -->
    <tags-list :tags="tags" alignment="left" style="width: 100%" />

    <!-- Full width, align width -->
    <tags-list :tags="tags" alignment="width" style="width: 100%" />

    <!-- 50% width, align left -->
    <tags-list :tags="tags" alignment="left" style="width: 75%" />

    <!-- 50% width, align width -->
    <tags-list :tags="tags" alignment="width" style="width: 50%" />
  </div>
</template>

<script>
import TagsList from "../components/TagsList.vue";

export default {
  name: "home-page",
  components: {
    TagsList,
  },
  data() {
    return {
      tags: [{ text: "текст1" }, { text: "текст2", icon: "mdi-circle" }],
    };
  },
};
</script>
