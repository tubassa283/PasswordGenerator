<template>
    <div class="form-container">
      <InputField
        label="Password to Check:"
        type="text"
        v-model="passwordToCheck"
        placeholder="Enter password"
        :inputId="'password-checker-input'"
      />
      <button @click="checkPasswordStrength" class="btn btn-primary">
        Check Password
      </button>
      <div v-if="passwordStrength !== null" class="result">
        <h3 :class="passwordStrengthClass">Password Strength: {{ passwordStrength }}</h3>
      </div>
    </div>
  </template>
  
  <script>
  import InputField from "./InputField.vue";
  
  export default {
    components: { InputField },
    data() {
      return {
        passwordToCheck: "",
        passwordStrength: null,
      };
    },
    computed: {
      passwordStrengthClass() {
        if (this.passwordStrength === "Strong") {
          return "strong";
        } else if (this.passwordStrength === "Medium") {
          return "medium";
        } else if (this.passwordStrength === "Weak") {
          return "weak";
        }
        return "";
      },
    },
    methods: {
      checkPasswordStrength() {
        const length = this.passwordToCheck.length;
        const hasUppercase = /[A-Z]/.test(this.passwordToCheck);
        const hasLowercase = /[a-z]/.test(this.passwordToCheck);
        const hasNumbers = /\d/.test(this.passwordToCheck);
        const hasSymbols = /[!@#$%^&*(),.?":{}|<>]/.test(this.passwordToCheck);
  
        if (length >= 12 && hasUppercase && hasLowercase && hasNumbers && hasSymbols) {
          this.passwordStrength = "Strong";
        } else if (length >= 8 && hasUppercase && hasLowercase && (hasNumbers || hasSymbols)) {
          this.passwordStrength = "Medium";
        } else {
          this.passwordStrength = "Weak";
        }
      },
    },
  };
  </script>
  
  <style scoped>
  .form-container {
    max-width: 600px;
    margin: 0 auto;
    padding: 20px;
    font-family: Arial, sans-serif;
  }
  
  .btn-primary {
    background: #007bff;
    padding: 12px;
    color: white;
    margin-top: 10px;
    width: 100%;
  }
  
  .result {
    margin-top: 20px;
    font-size: 1.2em;
    text-align: center;
  }
  
  .strong {
    color: green;
  }
  
  .medium {
    color: orange;
  }
  
  .weak {
    color: red;
  }
  </style>
  