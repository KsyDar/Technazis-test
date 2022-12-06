<template>
  <v-app>
    <v-main>
      <v-container fluid>
        <TextChips :tags="tags" align="justify" />
        <TextChips :tags="tags" class="" align="left" />
        <div class="d-flex">
          <TextChips :tags="tags" align="justify" class="w-50 mr-3" />
          <TextChips :tags="tags" align="left" class="w-50" />
        </div>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import TextChips from './components/TextChips.vue';

export default {
  name: 'App',

  components: {
    TextChips,
  },

  data() {
    return {
      tags: [
        {
          text: '5',
          icon: 'mdi-star-outline',
        },
        {
          text: 'Историческая',
        },
        {
          text: 'На автобусе',
          icon: 'mdi-bus',
        },
        {
          text: '28 марта',
        },
        {
          text: '9 часов',
          icon: 'mdi-clock-time-three-outline',
        },
        {
          text: 'до 49',
          icon: 'mdi-account',
        },
        {
          text: 'Обзорная',
        },
      ],
    };
  },
};
</script>

<style lang="scss" scoped>
.w-50 {
  width: 50%
}
</style>
