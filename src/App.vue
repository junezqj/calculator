<template>
  <div id="app">
    <div class="calculator">
      <div class="displayer">{{ str }}</div>
      <div class="tool">
        <button @click="getNumber">7</button>
        <button @click="getNumber">8</button>
        <button @click="getNumber">9</button>
        <button @click="getNumber">/</button>
        <button @click="getNumber">4</button>
        <button @click="getNumber">5</button>
        <button @click="getNumber">6</button>
        <button @click="getNumber">*</button>
        <button @click="getNumber">1</button>
        <button @click="getNumber">2</button>
        <button @click="getNumber">3</button>
        <button @click="getNumber">-</button>
        <button @click="clearNumber">AC</button>
        <button @click="getNumber">0</button>
        <button @click="getResult">=</button>
        <button @click="getNumber">+</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      str: "",
    };
  },
  mounted(){
    let exp = 'a+b*(c^d-e)^(f+g*h)-i'
    console.log(this.infixToPostfix(exp))
  },
  methods: {
    getNumber(e) {
      this.str += e.target.innerText;
    },
    getResult() {
      const arr = [];
      let num = "";
      for (let i = 0; i < this.str.length; i++) {
        const c = this.str[i];
        if (c === "+" || c === "-" || c === "*" || c === "/") {
          arr.push(num);
          arr.push(c);
          num = "";
        } else {
          num += c;
        }
      }
      arr.push(num);
      this.str = arr[0];
      for (let i = 1; i < arr.length; i += 2) {
        if (arr[i] === "+") {
          this.str = parseInt(this.str) + parseInt(arr[i + 1]);
        } else if (arr[i] === "-") {
          this.str = parseInt(this.str) - parseInt(arr[i + 1]);
        } else if (arr[i] === "*") {
          this.str = parseInt(this.str) * parseInt(arr[i + 1]);
        } else if (arr[i] === "/") {
          this.str = parseInt(this.str) / parseInt(arr[i + 1]);
        }
      }
    },
    clearNumber() {
      this.str = "";
    },
    infixToPostfix(s) {
      let st = [];
      let result = "";
      for (let i = 0; i < s.length; i++) {
        let c = s[i];

        if (
          (c >= "a" && c <= "z") ||
          (c >= "A" && c <= "Z") ||
          (c >= "0" && c <= "9")
        )
          result += c;
        else if (c === "(") st.push("(");
        else if (c === ")") {
          while (st[st.length - 1] !== "(") {
            result += st.pop();
          }
          st.pop();
        } else {
          while (st.length !== 0 && this.prec(s[i]) <= this.prec(st[st.length - 1])) {
            result += st.pop();
          }
          st.push(c);
        }
      }
      while (st.length !== 0) {
        result += st.pop();
      }
      return result;
    },
    prec(c) {
      if (c === "^") return 3;
      else if (c === "/" || c === "*") return 2;
      else if (c === "+" || c === "-") return 1;
      else return -1;
    },
  },
};
</script>

<style lang="scss">
.calculator {
  width: 480px;
  height: 580px;
  margin: 100px auto;
  border: 2px solid black;

  .displayer {
    width: 440px;
    height: 100px;
    line-height: 100px;
    font-size: 25px;
    color: red;
    margin: 10px auto;
    border: 1px solid black;
    border-radius: 10px;
    padding-left: 20px;
  }

  .tool {
    display: flex;
    justify-content: space-around;
    flex-wrap: wrap;

    button {
      width: 100px;
      height: 100px;
      line-height: 100px;
      text-align: center;
      font-size: 30px;
      background-color: pink;
      margin-top: 10px;
      border: 1px solid black;
      border-radius: 10px;
    }
  }
}
</style>
