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
        @update="updateParent"
      />
    </div>
  </div>
</template>

<script>
export default {
  name: "CategoryCheckbox",
  props: {
    node: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      checked: this.node.checked || false,
      indeterminate: false,
    };
  },
  watch: {
    'node.checked'(val) {
      this.checked = val;
    },
    checked(val) {
      this.$set(this.node, 'checked', val);
      this.updateIndeterminate();
    },
    'node.children': {
      handler() {
        this.updateIndeterminate();
        this.updateParent();
      },
      deep: true
    }
  },
  computed: {
    isIndeterminate() {
      if (!this.node.children || this.node.children.length === 0) {
        return false;
      }
      const checkedCount = this.node.children.filter(child => child.checked).length;
      return checkedCount > 0 && checkedCount < this.node.children.length;
    },
  },
  methods: {
    toggleCheck() {
      const newValue = !this.checked;
      this.checked = newValue;
      this.updateChildren(this.node.children, newValue);
      this.$emit('update');
    },
    updateChildren(children, checked) {
      if (children) {
        children.forEach(child => {
          this.$set(child, 'checked', checked);
          if (child.children) {
            this.updateChildren(child.children, checked);
          }
        });
      }
    },
    updateParent() {
      if (!this.node.children) return;
      const allChecked = this.node.children.every(child => child.checked);
      this.checked = allChecked;
      this.$set(this.node, 'checked', allChecked);
      this.updateIndeterminate();
      this.$emit('update');
    },
    updateIndeterminate() {
      this.indeterminate = this.isIndeterminate;
    }
  },
  mounted() {
    this.updateIndeterminate();
  }
};
</script>