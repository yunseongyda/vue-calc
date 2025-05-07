<script>
  export default {
    data() {
      return {
        output: null,
        prev: null,
        cur: null,
        operator: null,
        isDarkMode: false, // 다크모드 상태 추가
      };
    },

    methods: {
      userInput(n) {
        this.cur = this.cur === null ? n : (this.cur += n);
        this.output = this.cur;
      },

      carculate(n) {
        this.cur = Number(this.cur);
        if (this.operator !== null) {
          switch (this.operator) {
            case "+":
              this.prev = this.prev + this.cur;
              break;
            case "-":
              this.prev = this.prev - this.cur;
              break;
            case "*":
              this.prev = this.prev * this.cur;
              break;
            case "/":
              this.prev = this.prev / this.cur;
              break;
          }
          if (n === "=") {
            this.output = this.prev;
            this.operator = null;
            this.cur = this.prev;
          } else {
            this.output = null;
            this.operator = n;
            this.cur = null;
          }
        } else {
          this.output = null;
          this.operator = n;
          this.prev = this.cur;
          this.cur = null;
        }
        return;
      },

      clear() {
        this.output = null;
        this.prev = null;
        this.cur = null;
        this.operator = null;
      },

      operation(e) {
        const n = e.currentTarget.value;
        if (n === "C") {
          this.clear();
        } else if (["+", "-", "*", "/", "="].includes(n)) {
          this.carculate(n);
        } else {
          this.userInput(n);
        }
      },

      toggleDarkMode() {
        this.isDarkMode = !this.isDarkMode; // 다크모드 상태를 토글하는 메서드
      },
    },
    watch: {
      isDarkMode(newVal) {
        if (newVal) {
          document.body.classList.add("dark");
        } else {
          document.body.classList.remove("dark");
        }
      },
    },
  };


</script>

<template>
  <div :class="['wrapper', { dark: isDarkMode }]">
    <div :class="['calculator', { dark: isDarkMode }]"> <!-- 다크모드 클래스 적용 -->
    <form name="forms">
      <input v-model="output" type="text" name="output" readonly />
      <input type="button" class="clear" value="C" @click="operation" />
      <input type="button" class="operator" value="/" @click="operation" />
      <input type="button" value="1" @click="operation" />
      <input type="button" value="2" @click="operation" />
      <input type="button" value="3" @click="operation" />
      <input type="button" class="operator" value="*" @click="operation" />
      <input type="button" value="4" @click="operation" />
      <input type="button" value="5" @click="operation" />
      <input type="button" value="6" @click="operation" />
      <input type="button" class="operator" value="+" @click="operation" />
      <input type="button" value="7" @click="operation" />
      <input type="button" value="8" @click="operation" />
      <input type="button" value="9" @click="operation" />
      <input type="button" class="operator" value="-" @click="operation" />
      <input type="button" class="dot" value="." @click="operation" />
      <input type="button" value="0" @click="operation" />
      <input type="button" class="operator result" value="=" @click="operation" />
    </form>
    <button @click="toggleDarkMode" class="dark-mode-toggle"> <!-- 다크모드 토글 버튼 -->
      {{ isDarkMode ? "라이트 모드" : "다크 모드" }} <!-- 다크모드 상태에 따라 버튼 텍스트 변경 -->
    </button>
  </div>
  </div>
  
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  background-color: #ffffff;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

body.dark {
  background-color: #505050; /* 다크모드 배경색 */
}

.calculator {
  width: 287px;
  border: 1px solid #333;
  background-color: #ccc;
  padding: 5px;
}

.calculator.dark {
  background-color: #333; /* 다크모드 계산기 배경색 */
  color: #fff; /* 다크모드 폰트 색상 */
}

.calculator form {
  display: grid;
  grid-template-columns: repeat(4, 65px);
  grid-auto-rows: 65px;
  grid-gap: 5px;
}

.calculator form input {
  border: 2px solid #333;
  cursor: pointer;
  font-size: 19px;
  background-color: #f0f0f0; /* 기본 버튼 배경색 */
  color: #000; /* 기본 버튼 폰트 색상 */
}

.calculator.dark form input {
  background-color: #555; /* 다크모드 버튼 배경색 */
  color: #fff; /* 다크모드 버튼 폰트 색상 */
}

.calculator form input:hover {
  box-shadow: 1px 1px 2px #333;
}

.calculator form .clear {
  background-color: #ed4848;
  color: #fff;
}

.calculator.dark form .clear {
  background-color: #b22222; /* 다크모드 Clear 버튼 색상 */
}

.calculator form .operator {
  background-color: orange;
  color: #fff;
}

.calculator.dark form .operator {
  background-color: #ff8c00; /* 다크모드 연산자 버튼 색상 */
}

.calculator form .dot {
  background-color: green;
  color: #fff;
}

.calculator.dark form .dot {
  background-color: #006400; /* 다크모드 Dot 버튼 색상 */
}

.calculator form input[type="text"] {
  grid-column: span 4;
  text-align: right;
  padding: 0 10px;
  background-color: #fff; /* 기본 출력창 배경색 */
  color: #000; /* 기본 출력창 폰트 색상 */
}

.calculator.dark form input[type="text"] {
  background-color: #222; /* 다크모드 출력창 배경색 */
  color: #fff; /* 다크모드 출력창 폰트 색상 */
}

.calculator form .clear {
  grid-column: span 3;
}

.calculator form .result {
  grid-column: span 2;
}

.dark-mode-toggle {
  margin-top: 10px;
  padding: 10px;
  background-color: #555; /* 다크모드 토글 버튼 배경색 */
  color: #fff; /* 다크모드 토글 버튼 폰트 색상 */
  border: none;
  cursor: pointer;
}

.dark-mode-toggle:hover {
  background-color: #777; /* 다크모드 토글 버튼 호버 색상 */
}
</style>
