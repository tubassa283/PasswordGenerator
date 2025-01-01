<template>
  <div class="container">
    <!-- タイトル -->
    <h1 class="title">Password Generator</h1>

    <!-- タブ切り替え -->
    <div class="tabs">
      <button
        class="tab"
        :class="{ active: currentTab === 'generate' }"
        @click="currentTab = 'generate'"
      >
        Generate
      </button>
      <button
        class="tab"
        :class="{ active: currentTab === 'check' }"
        @click="currentTab = 'check'"
      >
        Check
      </button>
    </div>

    <!-- タブに応じた内容を表示 -->
    <div v-if="currentTab === 'generate'">
      <!-- パスワード生成フォーム -->
      <div class="form-container">
        <InputField
          label="Password Length:"
          type="number"
          v-model="passwordLength"
          :min="6"
          :max="32"
          :inputId="'password-length'"
        />
        <CheckboxField
          label="Include Uppercase:"
          v-model="includeUppercase"
          :inputId="'include-uppercase'"
        />
        <CheckboxField
          label="Include Lowercase:"
          v-model="includeLowercase"
          :inputId="'include-lowercase'"
        />
        <CheckboxField
          label="Include Numbers:"
          v-model="includeNumbers"
          :inputId="'include-numbers'"
        />
        <CheckboxField
          label="Include Symbols:"
          v-model="includeSymbols"
          :inputId="'include-symbols'"
        />
        <InputField
          label="Exclude Characters:"
          type="text"
          v-model="excludeChars"
          placeholder="e.g. abc"
          :inputId="'exclude-characters'"
        />
        <InputField
          label="Number of Passwords:"
          type="number"
          v-model="numberOfPasswords"
          :min="1"
          :max="10"
          :inputId="'number-of-passwords'"
        />
        <button @click="generatePassword" class="btn btn-primary">
          Generate Passwords
        </button>
      </div>

      <!-- 生成されたパスワードのリスト -->
      <PasswordList
        v-if="generatedPasswords.length > 0"
        :passwords="generatedPasswords"
        @download="downloadPasswords"
        @clear="clearPasswords"
      />
    </div>

    <div v-else>
      <!-- パスワード診断 -->
      <PasswordChecker />
    </div>
  </div>
</template>

<script>
import InputField from "./components/InputField.vue";
import CheckboxField from "./components/CheckboxField.vue";
import PasswordList from "./components/PasswordList.vue";
import PasswordChecker from "./components/PasswordChecker.vue";

export default {
  components: { InputField, CheckboxField, PasswordList, PasswordChecker },
  data() {
    return {
      currentTab: "generate", // 初期タブを生成に設定
      passwordLength: 6,
      includeUppercase: true,
      includeLowercase: true,
      includeNumbers: true,
      includeSymbols: true,
      excludeChars: "",
      numberOfPasswords: 1,
      generatedPasswords: [],
    };
  },
  methods: {
    generatePassword() {
      // 入力のバリデーション
      if (!this.includeUppercase && !this.includeLowercase && !this.includeNumbers && !this.includeSymbols) {
        alert("チェックボックスを選択してください。");
        return;
      }

      const chars = [];
      if (this.includeUppercase) chars.push("ABCDEFGHIJKLMNOPQRSTUVWXYZ");
      if (this.includeLowercase) chars.push("abcdefghijklmnopqrstuvwxyz");
      if (this.includeNumbers) chars.push("0123456789");
      if (this.includeSymbols) chars.push("!@#$%^&*()-_+=<>?");

      const allChars = chars.join("");
      const excludeSet = new Set(this.excludeChars);

      this.generatedPasswords = [];
      for (let i = 0; i < this.numberOfPasswords; i++) {
        let password = "";
        while (password.length < this.passwordLength) {
          const randomChar =
            allChars.charAt(Math.floor(Math.random() * allChars.length));
          if (!excludeSet.has(randomChar)) {
            password += randomChar;
          }
        }
        this.generatedPasswords.push(password);
      }
    },
    downloadPasswords() {
      const csvContent = this.generatedPasswords.join("\n");
      const blob = new Blob([csvContent], { type: "text/csv" });
      const link = document.createElement("a");
      link.href = URL.createObjectURL(blob);
      link.download = "passwords.csv";
      link.click();
    },
    clearPasswords() {
      this.generatedPasswords = [];
    },
  },
};
</script>

<style scoped>
.container {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}

.title {
  text-align: center;
  font-size: 3em;
  margin-bottom: 30px;
}

.tabs {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.tab {
  padding: 10px 30px;
  margin: 0 5px;
  background-color: #e0f7fa;
  border: 1px solid #ccc;
  border-radius: 5px;
  cursor: pointer;
  font-size: 18px;
}

.tab.active {
  background-color: #007bff;
  color: white;
  border: 1px solid #007bff;
}

.form-container {
  background: #e0f7fa;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.btn {
  display: block;
  width: 100%;
  padding: 10px;
  border: none;
  border-radius: 4px;
  font-size: 16px;
  cursor: pointer;
}

.btn-primary {
  background: #007bff;
  color: white;
  margin-top: 10px;
}
</style>
