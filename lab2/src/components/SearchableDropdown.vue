<script>
export default {
  name: 'SearchableDropdown',
  props: {
    options: {
      type: Array,
      required: true,
    },
    multiple: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      searchQuery: '',
      isOpen: false,
      selectedOptions: this.multiple ? [] : null,
      highlightedIndex: -1,
    };
  },
  computed: {
    filteredOptions() {
      const query = this.searchQuery.toLowerCase();
      return this.options.filter(item =>
        item.toLowerCase().includes(query)
      );
    },
  },
  methods: {
    navigate(step) {
      if (!this.isOpen) return;
      const maxIndex = this.filteredOptions.length - 1;
      this.highlightedIndex = (this.highlightedIndex + step + maxIndex + 1) % (maxIndex + 1);
    },
    selectOption(index = this.highlightedIndex) {
      if (index >= 0 && index < this.filteredOptions.length) {
        const selectedOption = this.filteredOptions[index];
        
        if (this.multiple) {
          const optionExists = this.selectedOptions.includes(selectedOption);
          if (!optionExists) {
            this.selectedOptions.push(selectedOption);
          } else {
            this.selectedOptions = this.selectedOptions.filter(option => option !== selectedOption);
          }
          this.$emit('update:modelValue', this.selectedOptions);
        } else {
          this.selectedOptions = selectedOption;
          this.$emit('update:modelValue', this.selectedOptions);
          this.isOpen = false;
          this.searchQuery = selectedOption;
        }
      }
    },
    clearSelection() {
      this.selectedOptions = this.multiple ? [] : null;
      this.searchQuery = '';
      this.$emit('update:modelValue', this.selectedOptions);
    },
  },
  watch: {
    selectedOptions(newValue) {
      if (newValue === null || (this.multiple && newValue.length === 0)) {
        this.isOpen = true;
      }
    },
  },
};
</script>

<template>
  <div class="relative">
    <input
      type="text"
      v-model="searchQuery"
      @focus="isOpen = true"
      @keydown.down.prevent="navigate(1)"
      @keydown.up.prevent="navigate(-1)"
      @keydown.enter.prevent="selectOption"
      @keydown.esc="isOpen = false"
      placeholder="Виберіть елемент"
      class="w-full border border-gray-300 rounded px-4 py-2 focus:outline-none focus:border-blue-500 transition duration-150 ease-in-out"
    />
    
    <button
      v-if="selectedOptions && (multiple ? selectedOptions.length > 0 : selectedOptions)"
      @click="clearSelection"
      class="absolute right-2 top-1 bottom-1 text-gray-500 hover:text-gray-700 transition duration-150 ease-in-out px-2 py-1 text-sm"
    >
      Очистити
    </button>

    <ul
      v-if="isOpen"
      class="absolute left-0 right-0 mt-2 bg-white border border-gray-300 rounded shadow-lg z-10 max-h-60 overflow-y-auto"
    >
      <li
        v-for="(item, index) in filteredOptions"
        :key="index"
        @click="selectOption(index)"
        :class="[
          'px-4 py-2 cursor-pointer',
          (multiple ? selectedOptions.includes(item) : item === selectedOptions) ? 'bg-blue-500 text-white' : 'text-black hover:bg-gray-100'
        ]"
        class="transition duration-150 ease-in-out"
      >
        <slot :item="item">{{ item }}</slot>
      </li>

      <li v-if="filteredOptions.length === 0" class="px-4 py-2 text-gray-500">
        Немає результатів
      </li>
    </ul>

    <!-- Display selected options if multiple -->
    <div v-if="multiple && selectedOptions.length > 0" class="mt-2">
      <div
        v-for="(selected, index) in selectedOptions"
        :key="index"
        class="inline-block bg-blue-100 text-blue-700 px-2 py-1 rounded-full mr-2 mb-2"
      >
        {{ selected }}
      </div>
    </div>
  </div>
</template>

<style scoped>
ul {
  max-height: 240px;
  overflow-y: auto;
}
li {
  user-select: none;
}
li:hover {
  background-color: #f1f1f1;
}
</style>
