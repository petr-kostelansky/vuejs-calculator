<template>
  <div class="calculator">
    <div class="history">
      <div v-for="(item, index) in history" v-bind:key="index">
        {{ item }}
      </div>
    </div>
    <div class="result">{{ statement + currentValue }}</div>
    <button @click="clear" class="button operator">AC</button>
    <button @click="reverse" class="button operator" :disabled="isDisabled">+/-</button>
    <button @click="addOperator('/')" class="button operator" :disabled="isDisabled">/</button>
    <button @click="addOperator('*')" class="button operator" :disabled="isDisabled">*</button> 
    <button @click="append('7')" class="button" :disabled="isDisabled">7</button>
    <button @click="append('8')" class="button" :disabled="isDisabled">8</button>
    <button @click="append('9')" class="button" :disabled="isDisabled">9</button>
    <button @click="addOperator('-')" class="button operator" :disabled="isDisabled">-</button> 
    <button @click="append('4')" class="button" :disabled="isDisabled">4</button>
    <button @click="append('5')" class="button" :disabled="isDisabled">5</button> 
    <button @click="append('6')" class="button" :disabled="isDisabled">6</button>
    <button @click="addOperator('+')" class="button operator" :disabled="isDisabled">+</button> 
    <button @click="append('1')" class="button" :disabled="isDisabled">1</button> 
    <button @click="append('2')" class="button" :disabled="isDisabled">2</button> 
    <button @click="append('3')" class="button" :disabled="isDisabled">3</button> 
    <button @click="append('0')" class="button zero" :disabled="isDisabled">0</button> 
    <button @click="dot" class="button" :disabled="isDisabled">.</button> 
    <button @click="equal" class="button operator equal" :disabled="isDisabled">=</button> 
  </div>
</template>

<script>
/* eslint-disable no-debugger */
export default {
  name: 'Calculator',
  data() {
    return {
      history: [],
      statement: "",
      currentValue: "",
      operatorAdded: false,
      isFirstCalculated: false,
      isDisabled: false
    };
  },
  methods: {
    clear() {
      this.history = [];
      this.statement = "";
      this.currentValue = "";
      this.operatorAdded = false;
      this.isFirstCalculated = false;
      this.isDisabled = false;
    },
    reverse() {
      if (this.currentValue != "") {
				this.currentValue = this.currentValue.charAt(0) === "-"
					? this.currentValue.slice(1)
					: `-${this.currentValue}`;
			}
    },
    dot() {
      if (this.currentValue && this.currentValue.indexOf(".") === -1) {
				this.append(".");
			}
    },
    equal() {
      if (this.operatorAdded && !this.currentValue) return;

      this.history.splice(0, 0, `${this.statement}${this.currentValue}`);
      this.statement = eval(this.statement + this.currentValue);

      if (!isFinite(this.statement) || isNaN(this.statement)) 
        this.isDisabled = true;

      this.isFirstCalculated = true;
      this.operatorAdded = false;
      this.currentValue = "";
    },
    addOperator(operator) {
      if (this.operatorAdded) return;

      this.operatorAdded = true;
      this.statement += `${this.currentValue} ${operator} `;
      this.currentValue = "";
    },
    append(number) {
      if (this.isFirstCalculated && !this.operatorAdded) return;

      if ((this.currentValue === "0" || this.currentValue === "-0") && number !== ".") {
        this.currentValue = "";
      }
      
      this.currentValue = `${this.currentValue}${number}`;
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.calculator {
  display: grid;
  grid-template-columns: 50px 50px 50px 50px;
  grid-template-rows: 80px 50px 50px 50px 50px 50px 50px;
  grid-gap: 5px;
  background-color: #C4DFE6;
  padding: 20px;
  border-radius: 5px;
}
.button {
  display: flex;
	justify-content: center;
	align-items: center;
	text-align: center;
	cursor: pointer;
  color: #07575B;
  background-color: #66A5AD;
  border-radius: 5px;
  border: 0;
}
button:disabled {
  cursor: initial;
  background-color: #C4DFE6;
  color: #66A5AD;
  border: 1px solid #66A5AD;
}
.operator {
  background-color: #07575B;
  color: #66A5AD;
}
.zero {
  grid-column: 1 / 3;
}
.equal {
  grid-row: 6 / 8;
  grid-column: 4;
}
.history {
  grid-column: 1 / 5;
  background-color: #66A5AD;
  color: #07575B;
  border-radius: 5px;
  padding: 5px;
  word-wrap: break-word;
  overflow-y: auto;
}
.result {
  grid-column: 1 / 5;
  background-color: #66A5AD;
  color: #07575B;
  border-radius: 5px;
  padding: 5px;
  word-wrap: break-word;
  overflow-y: auto;
}
</style>
