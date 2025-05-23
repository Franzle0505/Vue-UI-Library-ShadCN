<template>
  <div class="pb-3">
    <div class="w-full border-b">
      <div class="">
        <SearchMenu
          :modelValue="searchInput"
          class="pa-3"
          @update:model-value="
            (val) => {
              searchInput = val;
              val !== ''
                ? $emit('search:pexels', val, limit)
                : $emit('get:pexels', this.limit);
            }
          "
        />
      </div>
    </div>

    <scroll-area class="px-4" max-height="64vh">
      <div class="images__layout" v-if="images.length > 0">
        <div class="grid grid-cols-4 gap-3 py-3">
          <lazy v-for="image in images" :key="image.id" unrender>
            <img
              @click="
                $emit('select:image', {
                  id: image.id,
                  funnel_id: 0,
                  user_id: 0,
                  name: image.alt,
                  url: image.src.original,
                  path: image.src.original,
                })
              "
              :src="image.src.portrait"
              class="w-full pb-1"
            />
          </lazy>
        </div>

        <v-button
          v-if="limit < 80"
          class="mx-auto"
          color="primary"
          size="medium"
          @click="loadMore()"
        >
          Load More
        </v-button>
      </div>
      <div v-else class="d-flex justify-center pt-10 mt-10">
        <h6>
          <b> Image Not Found </b>
        </h6>
      </div>
    </scroll-area>
  </div>
</template>

<script>
import { defineAsyncComponent } from "vue";
import { RiSearchLine } from "vue-remix-icons";

export default {
  components: {
    RiSearchLine,
    ScrollArea: defineAsyncComponent(() =>
      import("@/components/shadcn/scroll-area/ScrollArea.vue")
    ),
    SearchMenu: defineAsyncComponent(() =>
      import("@/components/base/SearchMenu")
    ),
    Lazy: defineAsyncComponent(() => import("@/components/base/Lazy")),
    ImageCard: defineAsyncComponent(() => import("./ImageCard.vue")),
    VButton: defineAsyncComponent(() => import("@/components/base/VButton")),
  },
  props: {
    images: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      searchInput: "",
      limit: 40,
    };
  },

  mounted() {
    this.$emit("get:pexels", 40);
  },

  methods: {
    loadMore() {
      if (this.limit <= 80) {
        this.limit += 20;
        if (this.searchInput == "") {
          this.$emit("get:pexels", this.limit);
        } else {
          this.$emit("search:pexels", this.searchInput, this.limit);
        }
      }
    },
  },
};
</script>
<style lang="scss" scoped>
.images__layout {
  img {
    cursor: pointer;
    transition: all 0.3s ease;
    &:hover {
      scale: 1.03;
    }
  }
}
</style>
