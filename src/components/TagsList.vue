<template>
  <div
    ref="tagsList"
    class="tags-list"
    :class="{ 'tags-list--align-width': alignment === 'width' }"
  >
    <v-btn
      v-for="(tag, index) in displayedTags"
      :key="index"
      :outlined="true"
      class="tags-list__tag"
    >
      <v-icon v-if="tag.icon">
        {{ tag.icon }}
      </v-icon>

      <span>
        {{ tag.text }}
      </span>

      <v-icon v-if="isIconInLastTagShown(index)"> mdi-circle-small </v-icon>
    </v-btn>
  </div>
</template>

<script>
import { throttle } from "throttle-debounce";

export default {
  name: "tags-list",
  props: {
    tags: {
      type: Array,
      required: true,
    },
    alignment: {
      type: String,
      default: "left",
    },
  },
  data() {
    return {
      containerWidth: 0,
      throttledUpdateContainerWidth: null,
    };
  },
  created() {
    this.throttledUpdateContainerWidth = throttle(
      200,
      this.updateContainerWidth
    );
  },
  mounted() {
    this.$nextTick(() => {
      this.updateContainerWidth();

      window.addEventListener("resize", this.throttledUpdateContainerWidth);
    });
  },
  beforeDestroy() {
    window.removeEventListener("resize", this.throttledUpdateContainerWidth);
  },
  computed: {
    displayedTags() {
      let widthSum = 0;
      let tags = [];

      for (const tag of this.tags) {
        const tagWidth = this.getTagWidth(tag);

        if (widthSum + tagWidth <= this.containerWidth) {
          tags.push(tag);
          widthSum += tagWidth;
        } else {
          break;
        }
      }

      return tags;
    },
  },
  methods: {
    isIconInLastTagShown(index) {
      return index !== this.displayedTags.length - 1;
    },

    updateContainerWidth() {
      this.containerWidth = this.$refs.tagsList.clientWidth;
    },

    getTagWidth(tag) {
      const tagTextWidth = tag.text.length * 10;
      const iconWidth = tag.icon ? 24 : 0;
      const separatorWidth = 8;
      const paddingInTagCount = 32;

      return tagTextWidth + iconWidth + separatorWidth + paddingInTagCount;
    },
  },
};
</script>

<style lang="scss" scoped>
.tags-list {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 8px;

  &--align-width {
    justify-content: space-between;
  }

  &__tag {
    display: flex;
    align-items: center;
  }
}
</style>
