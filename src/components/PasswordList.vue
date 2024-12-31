<template>
    <div class="password-list">
      <h2>-----Generated Passwords-----</h2>
      <!-- ボタンを横並びに配置する -->
      <div class="button-container">
        <button @click="$emit('download')" class="btn btn-secondary">
          Download CSV
        </button>
        <button @click="$emit('clear')" class="btn btn-danger">
          Clear Passwords
        </button>
      </div>
      <!-- パスワードリスト -->
      <ul>
        <li v-for="(password, index) in passwords" :key="index">
          {{ password }}
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      passwords: Array,
    },
  };
  </script>
  
  <style scoped>
  .password-list {
    margin-top: 20px;
    text-align: center; /* パスワードリスト全体を中央揃え */
  }
  
  .password-list h2 {
    margin-bottom: 20px;
  }
  
  .button-container {
    display: flex;
    justify-content: center;
    gap: 10px; /* ボタンの間隔 */
    margin-bottom: 20px;
  }
  
  .password-list ul {
    list-style: none;
    padding: 0;
    margin: 0;
    display: flex;
    flex-direction: column;
    align-items: center; /* パスワードを中央揃え */
  }
  
  .password-list li {
    background: #f1f1f1;
    padding: 10px;
    margin-bottom: 5px;
    border-radius: 4px;
    width: 80%; /* パスワードリストの幅を調整 */
    text-align: center; /* パスワードを中央揃え */
  }
  
  .btn-secondary {
    background: #28a745;
    color: white;
    padding: 10px 20px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }
  
  .btn-danger {
    background: #dc3545;
    color: white;
    padding: 10px 20px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }
  
  .btn-secondary:hover,
  .btn-danger:hover {
    opacity: 0.8; /* ホバー時に少し透明にする */
  }
  </style>
  