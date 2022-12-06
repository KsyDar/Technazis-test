<template>
  <div
    :class="'justify-' + rowAlign"
    class="d-flex flex-nowrap overflow-hidden"
    ref="parent"
  >
    <template v-for="(tag, index) in renderItems">
      <div
        :key="'item' + tag.id"
        class="d-flex flex-row flex-nowrap align-center pa-0"
        ref="children"
      >
        <v-icon v-if="tag.icon" class="mr-2"> {{ tag.icon }}</v-icon>
        <span class="tag-text">{{ tag.text }}</span>
      </div>
      <div
        :key="'icon'+ tag.id"
        v-if="index !== renderItems.length - 1"
        is-icon
        ref="children"
      >
        <v-icon> mdi-circle-small </v-icon>
      </div>
    </template>
  </div>
</template>

<script>
import { v4 } from 'uuid';

export default {
  name: 'TextChips',
  props: {
    /** теги */
    tags: {
      type: Array,
      require: true,
    },
    /** тип выравнивания */
    align: {
      type: String,
      default: 'left',
      validator: (val) => ['left', 'justify'].indexOf(val) !== -1,
    },
  },

  data() {
    return {
      /** используется для отрисовки только тех элементов, которые помещаются в родителя */
      renderItems: [],
      /** наблюдать за изменениями размеров родетеля */
      resizeObserver: null,
      /** внутренне поля для хранения тегов */
      internalTags: [],
    };
  },

  watch: {
    tags: {
      handler() {
        this.internalTags = this.tags.map((tag) => ({ id: v4(), ...tag }));
      },
      immediate: true,
    },
  },

  mounted() {
    const { parent } = this.$refs;
    this.resizeObserver = new ResizeObserver((entries) => {
      this.renderItems = this.internalTags;

      this.$nextTick(() => {
        const { children } = this.$refs;
        const parentWidth = entries[0].contentRect.width;
        let size = 0;
        let index = 0;
        let lastItemIndex = 0;
        while (size <= parentWidth) {
          const child = children[index];
          if (child === undefined) {
            lastItemIndex += 1;
            break;
          }
          size += child.clientWidth;
          index += 1;
          if (child.attributes['is-icon'] === undefined) lastItemIndex += 1;
        }
        this.renderItems = this.internalTags.slice(0, lastItemIndex - 1);
      });
    });
    this.resizeObserver.observe(parent);
  },

  destroyed() {
    this.resizeObserver.disconnect();
  },

  computed: {
    /**
     * Преобразует входной параметр align в формат класса
     */
    rowAlign() {
      const classNames = {
        left: 'start',
        justify: 'space-between',
      };
      return classNames[this.align];
    },
  },
};
</script>

<style scoped lang="scss">
.tag-text {
  white-space: nowrap;
}

</style>
