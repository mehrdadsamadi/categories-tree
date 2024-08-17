<template>
  <div></div>
</template>

<script>
import categoriesList from '@/constant/categories-tree-list';

export default {
  data() {
    return {
      categories: categoriesList
    };
  },
  created() {
    console.log(this.categories);
    
  },
  methods: {
  }
};
</script>