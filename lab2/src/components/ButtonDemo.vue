<script setup>
import Button from './Button.vue'
</script>

<template>
  <div class="flex items-center justify-center">
    <div class="mr-2">
      <Button label="Default Button" @click="handleClick" />
    </div>
    <div class="mr-2">
      <Button label="Primary Action" color="blue" size="medium" @click="handleClick" />
    </div>
    <div class="mr-2">
      <Button label="Delete" color="red" size="large" @click="handleClick" />
    </div>
    <div class="mr-2">
      <Button label="Disabled" color="gray" size="small" :disabled="true" @click="handleClick" />
    </div>
    <div class="mr-2">
      <Button label="Download" color="green" size="medium" icon="fas fa-download" @click="handleClick" />
    </div>
    <div class="mr-2">
      <Button label="Edit" color="purple" size="large" icon="fas fa-edit" @click="handleClick" />
    </div>
    <div class="mr-2">
      <Button color="blue" size="small" icon="fas fa-info-circle" @click="handleClick" />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      buttonLabel: 'Dynamic Button',
      buttonType: 'button',
      isButtonDisabled: false,
    };
  },
  methods: {
    handleClick() {
      console.log('Button clicked');
    },
  },
};
</script>
