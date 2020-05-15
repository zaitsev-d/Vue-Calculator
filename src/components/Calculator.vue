<template>
  <div>
    <form name="form">
      <header class="display element">
        <div class="text a">{{ result || '' }}</div>
        <div class="text b">{{ current || '0' }}</div>
      </header>
    </form>
    <table class="element">
      <tr>
        <td><input class="button operator_alt" type="button" value="C" @click="clear"></td>
        <td><input class="button operator_alt" type="button" value="+/-" @click="sign"></td>
        <td><input class="button operator_alt" type="button" value="%" @click="percent"></td>
        <td><input class="button operator" type="button" value="÷" @click="divide"></td>
      </tr>
      <tr>
        <td><input class="button" type="button" value="7" @click="append('7')"></td>
        <td><input class="button" type="button" value="8" @click="append('8')"></td>
        <td><input class="button" type="button" value="9" @click="append('9')"></td>
        <td><input class="button operator" type="button" value="x" @click="times"></td>
      </tr>
      <tr>
        <td><input class="button" type="button" value="4" @click="append('4')"></td>
        <td><input class="button" type="button" value="5" @click="append('5')"></td>
        <td><input class="button" type="button" value="6" @click="append('6')"></td>
        <td><input class="button operator" type="button" value="-" @click="minus"></td>
      </tr>
      <tr>
        <td><input class="button" type="button" value="1" @click="append('1')"></td>
        <td><input class="button" type="button" value="2" @click="append('2')"></td>
        <td><input class="button" type="button" value="3" @click="append('3')"></td>
        <td><input class="button operator" type="button" value="+" @click="plus"></td>
      </tr>
      <tr>
        <td colspan=2><input class="button" style="width: 105px" type="button" value="0" @click="append('0')"></td>
        <td><input class="button operator_cyan" type="button" value="." @click="dot"></td>
        <td><input class="button operator" type="button" value="=" @click="equal"></td>
      </tr>
    </table>  
  </div>
</template>

<script>
export default {
  data() {
    return {
      previous: null,
      current: '',
      currentOperator: '',
      result: '',
      operator: null,
      operatorClicked: false
    }
  },
  methods: {
    clear(){
      this.current = '',
      this.currentOperator='',
      this.result=''
    },
    sign() {
      this.current = this.current.charAt(0) === '-' ?
      this.current.slice(1) : `-${this.current}`
    },
    percent() {
      this.current = `${parseFloat(this.current) / 100}`
    },
    append(number){
      if(this.operatorClicked){
        this.current = ''
        this.operatorClicked = false
      }

      if(this.current.length <= 10) {this.current = `${this.current}${number}`}
    },
    dot(){
      if(this.current.indexOf('.') === -1){
        this.append('.');
      }
    },
    setPrevious(){
      this.previous = this.current
      this.operatorClicked = true
    },
    divide(){
      this.operator = (a, b) => a / b
      this.setPrevious()
      this.currentOperator = '÷'
      this.result = `${parseFloat(this.previous)} ${this.currentOperator} `
    },
    times(){
      this.operator = (a, b) => a * b
      this.setPrevious()
      this.currentOperator = 'x'
      this.result = `${parseFloat(this.previous)} ${this.currentOperator} `
    },
    minus(){
      this.operator = (a, b) => a - b
      this.setPrevious()
      this.currentOperator = '-'
      this.result = `${parseFloat(this.previous)} ${this.currentOperator} `
    },
    plus(){
      this.operator = (a, b) => a + b
      this.setPrevious()
      this.currentOperator = '+'
      this.result = `${parseFloat(this.previous)} ${this.currentOperator} `
    },
    equal(){
      this.result = `${parseFloat(this.previous)} ${this.currentOperator} ${parseFloat(this.current)} = `
      this.current = `${ this.operator(parseFloat(this.current), parseFloat(this.previous)) }`
      this.previous = null
    }
  }
}
</script>

<style scoped>

.display{
  width: 213px;
  height: 100px;
  padding: 5;
  background-color: black;
}
.text{
  height: 30px;
  font-size: 30px;
  text-align: right;
  color: white;
  padding: 8px;
}

.a{
  font-size: 20px;
  color: gray;
}

.element{
  margin-left: auto;
  margin-right: auto;
}

.button{
  width: 50px;
  height: 50px;
  font-family: 'Courier New', Courier, monospace;
  font-size: 20px;
  background-color:#252525;
  color: white;
  cursor: pointer;
  border: none;
}

.operator{
  background-color: #ffc309;
  color: #7b5904;
}

.operator_alt{
  background-color:#ff01b3;
  color: white;
}

.operator_cyan{
  background-color:#01eeff;
  color: white;
}
</style>
