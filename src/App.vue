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
// import categoriesList from '@/constant/categories-tree-list';
import categoriesFlatList from '@/constant/categories-flat-list';
import CategoryCheckbox from './components/CategoryCheckbox.vue';

export default {
  name: 'App',
  components: {
    CategoryCheckbox
  },
  data() {
    return {
      categories: []
    }
  },
  created() {
    this.categories = this.treeCategories
  },
  computed: {
    treeCategories() {
      return this.convertToTree(categoriesFlatList);
    }
  },
  methods: {
    convertToTree(cats, pId = undefined) {
      return cats.filter(category => category.parentId === pId).map(category => ({
        ...category,
        children: this.convertToTree(cats, category.id)
      }));
    }
  },
}
</script>