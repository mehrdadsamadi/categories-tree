<template>
    <div>
        <label class="flex items-center flex-row-reverse gap-2">
            <input
                type="checkbox"
                :checked="checked"
                :indeterminate.prop="indeterminate"
                @change="toggleCheck"
            />
            <span class="font-semibold">{{ node.name }}</span>
        </label>
        <div v-if="node.children" class="me-8">
            <CategoryCheckbox
                v-for="cat in node.children" 
                :key="cat.id"
                :node="cat"
                @update="toggleCheck"
            />
        </div>
    </div>
  </template>
  
<script>

  export default {
    name:"CategoryCheckbox",
    data() {
      return {
        checked: false,
        indeterminate: false,
      };
    },
    props: {
        node: {
          type: Object,
          required: true,
        },
    },
    computed: {
        isIndeterminate() {
          return this.node.children && this.node.children.some(child => child.checked !== this.checked);
        },
    },
    methods: {
        toggleCheck() {
          this.checked = !this.checked;
          this.updateChildren(this.node.children, this.checked);
          this.$emit('update');
        },
        updateChildren(children, checked) {
          if (children) {
            children.forEach(child => {
              child.checked = checked;
              if (child.children) {
                this.updateChildren(child.children, checked);
              }
            });
          }
        },
    },
    watch: {
        checked() {
          this.indeterminate = this.isIndeterminate;
        },
      },
  };
</script>
  