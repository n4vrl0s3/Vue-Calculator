<template>
  <div class="container">
    <div class="box-calculator">
      <div id="display-calculator">
        <div class="history-calculator">
          <div v-for="item in history" :key="item.operation">
            <p>{{ item.operation }}</p>
            <p>= {{ item.resultOperation }}</p>
            <br />
          </div>
        </div>
        <div class="result-calculator">
          {{ currentCalculator }}
        </div>
      </div>
      <div class="buttons-calculator">
        <div class="row">
          <button @click="deleteAll()">AC</button>
          <button @click="deleteCharactor()">DEL</button>
          <button @click="getPercent()">%</button>
          <button @click="input('/')">÷</button>
        </div>
        <div class="row">
          <button @click="input('7')">7</button>
          <button @click="input('8')">8</button>
          <button @click="input('9')">9</button>
          <button @click="input('*')">x</button>
        </div>
        <div class="row">
          <button @click="input('4')">4</button>
          <button @click="input('5')">5</button>
          <button @click="input('6')">6</button>
          <button @click="input('-')">-</button>
        </div>
        <div class="row">
          <button @click="input('3')">3</button>
          <button @click="input('2')">2</button>
          <button @click="input('1')">1</button>
          <button @click="input('+')">+</button>
        </div>
        <div class="row">
          <button @click="input('0')">0</button>
          <button @click="input('.')">.</button>
          <button @click="deleteHistory()">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              height="1em"
              viewBox="0 0 448 512"
            >
              <path
                d="M170.5 51.6L151.5 80h145l-19-28.4c-1.5-2.2-4-3.6-6.7-3.6H177.1c-2.7 0-5.2 1.3-6.7 3.6zm147-26.6L354.2 80H368h48 8c13.3 0 24 10.7 24 24s-10.7 24-24 24h-8V432c0 44.2-35.8 80-80 80H112c-44.2 0-80-35.8-80-80V128H24c-13.3 0-24-10.7-24-24S10.7 80 24 80h8H80 93.8l36.7-55.1C140.9 9.4 158.4 0 177.1 0h93.7c18.7 0 36.2 9.4 46.6 24.9zM80 128V432c0 17.7 14.3 32 32 32H336c17.7 0 32-14.3 32-32V128H80zm80 64V400c0 8.8-7.2 16-16 16s-16-7.2-16-16V192c0-8.8 7.2-16 16-16s16 7.2 16 16zm80 0V400c0 8.8-7.2 16-16 16s-16-7.2-16-16V192c0-8.8 7.2-16 16-16s16 7.2 16 16zm80 0V400c0 8.8-7.2 16-16 16s-16-7.2-16-16V192c0-8.8 7.2-16 16-16s16 7.2 16 16z"
              />
            </svg>
          </button>
          <button @click="calculate()">=</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      currentCalculator: "0",
      history: [],
      lastOperator: "0",
    };
  },
  methods: {
    deleteAll() {
      this.currentCalculator = "0";
      this.lastOperation = "0";
    },
    deleteCharactor() {
      if (
        this.currentCalculator == "0" ||
        this.currentCalculator == "NaN" ||
        this.currentCalculator == "Infinity" ||
        this.currentCalculator == ""
      ) {
        this.currentCalculator = "0";
      } else {
        this.currentCalculator = this.currentCalculator.toString().slice(0, -1);
      }
    },
    getPercent() {
      if (this.lastIsOperand(this.currentCalculator))
        return this.currentCalculator;
      else {
        return (this.currentCalculator = eval(
          this.currentCalculator / 100
        ).toString());
      }
    },
    input(value) {
      if (value == ".") {
        let operators = ["+", "-", "x", "÷"];
        let lastIndex = Math.max(
          ...operators.map((operator) =>
            this.currentCalculator.lastIndexOf(operator)
          )
        );
        let result = this.currentCalculator.substring(lastIndex + 1).trim();
        if (result.includes(".")) {
          this.currentCalculator = this.currentCalculator
            .toString()
            .replace("*", "x");
        } else {
          this.currentCalculator += value;
        }
      } else if (value == "*" || value == "+" || value == "-" || value == "/") {
        if (this.lastIsOperand(this.currentCalculator)) {
          this.currentCalculator =
            this.currentCalculator.toString().slice(0, -1) + value;
        } else {
          this.currentCalculator += value;
        }
      } else if (
        this.currentCalculator == "NaN" ||
        this.currentCalculator == "Infinity"
      ) {
        return (this.currentCalculator = this.currentCalculator
          .toString()
          .replaceAll("*", "x")
          .replaceAll("/", "÷"));
      } else if (this.currentCalculator === "0") {
        this.currentCalculator = this.currentCalculator.slice(0, -1);
        this.currentCalculator += value;
      } else {
        this.currentCalculator += value;
      }
      return (this.currentCalculator = this.currentCalculator
        .toString()
        .replaceAll("*", "x")
        .replaceAll("/", "÷"));
    },
    calculate() {
      let result;
      this.currentCalculator = this.currentCalculator
        .replaceAll("x", "*")
        .replaceAll("÷", "/");
      if (
        !this.isIncludesOperand(this.currentCalculator) ||
        this.lastIsOperand(this.currentCalculator)
      ) {
        result = this.currentCalculator;
      } else if (this.currentCalculator.slice(-1) == ".") {
        result = this.currentCalculator;
      } else {
        result = eval(this.currentCalculator);
        if (typeof result === "number" && !Number.isInteger(result)) {
          result = result.toFixed(8);
        }
        this.addResultHistory(this.currentCalculator, result.toString());
      }
      this.currentCalculator = result.toString();
      this.currentCalculator = this.currentCalculator
        .toString()
        .replaceAll("*", "x")
        .replaceAll("/", "÷");
    },
    lastIsOperand(currentCalculate) {
      let operand = currentCalculate.slice(-1);
      return (
        operand == "+" || operand == "-" || operand == "x" || operand == "÷"
      );
    },
    isIncludesOperand(calculation) {
      const operands = ["+", "-", "*", "/"];
      return operands.some((o) => calculation.includes(o));
    },
    addResultHistory(operation, result) {
      this.history.push({
        operation: operation,
        resultOperation: result,
      });
    },
    deleteHistory() {
      this.history = [];
      this.deleteAll();
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
* {
  overflow: hidden;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.container {
  position: relative;
  width: 100%;
}
.box-calculator {
  margin: auto;
  border: 1px solid rgb(211, 143, 42);
  padding: 20px 20px 40px 20px;
  width: 350px;
  height: 620px;
  border-radius: 15px;
  background-color: rgb(180, 125, 151);
}
#display-calculator {
  height: 35%;
  padding: 10px 20px;
  background-color: rgb(244, 240, 240);
  border-radius: 15px;
  margin-bottom: 20px;
}
.history-calculator {
  height: 80%;
  text-align: end;
  overflow-y: auto;
}
.history-calculator::-webkit-scrollbar {
  width: 8px;
  border-radius: 4px;
  background-color: #fff;
  margin-left: 10px;
  border: 0.3px solid black;
}
.history-calculator::-webkit-scrollbar-thumb {
  background-color: #b48d8d;
}
.result-calculator {
  text-align: end;
  font-size: 30px;
  padding-right: 10px;
}
.buttons-calculator {
  background-color: rgb(244, 240, 240);
  padding: 10px 0;
  border-radius: 15px;
}
.buttons-calculator button {
  width: 60px;
  height: 60px;
  border-radius: 50%;
  margin: 4px 7px;
  border: 1px solid rgb(211, 143, 42);
  background-color: #fff;
  font-size: 20px;
  font-weight: 550;
  color: rgb(123, 120, 120);
  cursor: pointer;
}
</style>
