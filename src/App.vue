<template>
  <div id="app" class="p-4" >
    <CategoryCheckbox
      v-for="cat in categories" 
      :key="cat.id"
      :node="cat"
    />
  </div>
</template>

<script>
import categoriesList from '@/constant/categories-tree-list';
import CategoryCheckbox from './components/CategoryCheckbox.vue';

export default {
  name: 'App',
  components: {
    CategoryCheckbox
  },
  data() {
    return {
      categories: categoriesList
    }
  },
}
</script>