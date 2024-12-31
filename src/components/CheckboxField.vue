<template>
    <div class="input-group">
      <label>{{ label }}</label>
      <input type="checkbox" v-model="localValue" class="checkbox" />
    </div>
  </template>
  
  <script>
  export default {
    props: {
      label: String,
      modelValue: Boolean,
    },
    computed: {
      localValue: {
        get() {
          return this.modelValue;
        },
        set(value) {
          this.$emit("update:modelValue", value);
        },
      },
    },
  };
  </script>
  
  <style scoped>
  .input-group {
    display: flex;
    align-items: center;
    justify-content: space-between; /* ラベルとチェックボックスを左右に分ける */
    margin-bottom: 15px;
  }
  
  .input-group label {
    font-weight: bold;
    margin-right: 10px;
    text-align: left;
  }
  
  .checkbox {
    transform: scale(1.5); /* チェックボックスを大きくする */
  }
  </style>
  