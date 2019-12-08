<template>
  <div class="calculator">
    <div class="history">
      <div v-for="item in history" v-bind:key="item">
        {{ item }}
      </div>
    </div>
    <div class="result">{{ statement + currentValue }}</div>
    <div @click="clear" class="button operator">AC</div>
    <div @click="reverse" class="button operator">+/-</div>
    <div @click="addOperator('/')" class="button operator">/</div>
    <div @click="addOperator('*')" class="button operator">*</div> 
    <div @click="append('7')" class="button">7</div>
    <div @click="append('8')" class="button">8</div>
    <div @click="append('9')" class="button">9</div>
    <div @click="addOperator('-')" class="button operator">-</div> 
    <div @click="append('4')" class="button">4</div>
    <div @click="append('5')" class="button">5</div> 
    <div @click="append('6')" class="button">6</div>
    <div @click="addOperator('+')" class="button operator">+</div> 
    <div @click="append('1')" class="button">1</div> 
    <div @click="append('2')" class="button">2</div> 
    <div @click="append('3')" class="button">3</div> 
    <div @click="append('0')" class="button zero">0</div> 
    <div @click="dot" class="button">.</div> 
    <div @click="equal" class="button operator equal">=</div> 
  </div>
</template>

<script>
export default {
  name: 'Calculator',
  data() {
    return {
      history: [],
      statement: "",
      currentValue: "",
      operatorAdded: false,
      isFirstCalculated: false
    };
  },
  methods: {
    clear() {
      this.history = [];
      this.statement = "";
      this.currentValue = "";
      this.operatorAdded = true;
      this.isFirstCalculated = false;
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

      if (this.currentValue === "0" && number !== ".") {
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
