<template lang="html">
  <div class="calculator">
    <div id="display">{{ current || '0' }}</div>
    <div @click="clear" class="btn clear">C</div>
    <div @click="sign" class="btn">+/-</div>
    <div @click="percent" class="btn">%</div>
    <div @click="divide" class="btn operator">/</div>
    <div @click="append(7)" class="btn">7</div>
    <div @click="append(8)" class="btn">8</div>
    <div @click="append(9)" class="btn">9</div>
    <div @click="times" class="btn operator">x</div>
    <div @click="append(4)" class="btn">4</div>
    <div @click="append(5)" class="btn">5</div>
    <div @click="append(6)" class="btn">6</div>
    <div @click="minus" class="btn operator">-</div>
    <div @click="append(1)" class="btn">1</div>
    <div @click="append(2)" class="btn">2</div>
    <div @click="append(3)" class="btn">3</div>
    <div @click="add" class="btn operator">+</div>
    <div @click="append(0)" class="btn" id="zero">0</div>
    <div @click="dot" class="btn">.</div>
    <div @click="equal" class="btn equal">=</div>
  </div>
</template>

<script>
export default {
  data(){
    return {
      current: '',
      operator: null,
      previous: null,
      currentOperator: false
    }
  },
  methods: {
    clear(){
      this.current = ''
    },
    sign(){
      if (this.current !== '' && this.current !== '0') {
        if (this.current.charAt(0) === '-') {
          this.current = this.current.slice(1)
        } else {
          this.current = `-${this.current}`
        }
      }
    },
    percent(){
      this.current = `${parseFloat(this.current) / 100}`
    },
    append(number){
      if (this.currentOperator) {
        this.current = ''
        this.currentOperator
      }

      if ((this.current !== '' || this.current.charAt(0) === '0') || number != 0)
        this.current = `${this.current}${number}`
    },
    dot(){
      if (this.current.indexOf('.') === -1)
        this.append('.');
    },
    setPrevious(){
      this.previous = this.current
      this.currentOperator = true
    },
    divide(){
      this.operator = (a, b) => a / b
      this.setPrevious()
    },
    times(){
      this.operator = (a, b) => a * b
      this.setPrevious()
    },
    minus(){
      this.operator = (a, b) => a - b
      this.setPrevious()
    },
    add(){
      this.operator = (a, b) => a + b
      this.setPrevious()
    },
    equal(){
      this.current = `${this.operator(parseFloat(this.current), parseFloat(this.previous))}`
      this.previous = null
    }
  }
}
</script>

<style scoped>
.calculator {
  width: 350px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(50px, auto);
  font-size: 2.2rem;
}

.btn{
  -webkit-user-select: none;  /* Chrome all / Safari all */
  -moz-user-select: none;     /* Firefox all */
  -ms-user-select: none;      /* IE 10+ */
  user-select: none;          /* Likely future */  
  border: 1px solid #999;
  cursor: pointer;
  padding: .5rem
}

.btn.clear{
  color: orange
}

.btn.operator{
  background-color: orange;
  color: white;
}

.btn.equal{
  background-color: #333;
  color: white;
  border: 1px solid #333;
}

#display{
  background-color: #333;
  color: white;
  grid-column: 1 / 5;
  padding: .5rem
}

#zero{
  grid-column: 1 / 3;
}
</style>
