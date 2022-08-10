<template>
  <div>
    <h3>input expression</h3>
    <MathInput/>
    <br/>
    <button @click="solve">Solve</button>
    <br/>
    <div :class="{ hidden: !showSolution, shown: showSolution }">
      <h4>Input: </h4>
      <p>{{ inputText }}</p>
      <br/>
      <h4>Solution: </h4>
      <p>{{ solutionText }}</p>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import MathInput from '@/components/MathInput.vue'

export default defineComponent({
  name: 'ExpressionCalc',
  props: {
  },
  components: {
    MathInput
  },
  data () {
    return {
      showSolution: false,
      inputText: '',
      solutionText: '',
      expressionInput: ''
    }
  },
  methods: {

    // A function that takes in an expression as a string and returns the answer as a string
    // There is no error handeling, so if you input an invalid expression like 3**4^2 you will get an unexpected answer.
    // prob: {string}
    // return: {string}
    calculateExpression2: function (prob: string) {
      let out = prob

      // Part 1
      let depth = 0
      let buf = ''
      let inPar = false
      for (let i = 0; i < out.length; i++) {
        if (out[i] === '(') {
          if (depth === 0) inPar = true
          depth += 1
        }
        if (inPar === true) {
          buf = buf.concat(out[i])
        }
        if (out[i] === ')') {
          depth -= 1
          if (depth === 0) {
            inPar = false
            const bufSolved = this.calculateExpression(buf.slice(1, buf.length - 1))
            out = out.replace(buf, bufSolved)
            buf = ''
            i = 0
          }
        }
      }
      // Part 1

      // Part 2
      const opperators = ['\\^', '[\\*\\/]', '[\\+\\-]']
      for (let j = 0; j < opperators.length; j++) {
        while (true) {
          const powerRegex = new RegExp('(-?[0-9\\.]+) *(' + opperators[j] + ') *(-?[0-9\\.]+)')
          const opperation = out.match(powerRegex)
          if (opperation === null) break

          const num1 = parseFloat(opperation[1])
          const num2 = parseFloat(opperation[3])
          let calculatedNum = ''
          if (opperation[2] === '^') {
            calculatedNum = Math.pow(num1, num2).toString()
          } else if (opperation[2] === '*') {
            calculatedNum = (num1 * num2).toString()
          } else if (opperation[2] === '/') {
            calculatedNum = (num1 / num2).toString()
          } else if (opperation[2] === '+') {
            calculatedNum = (num1 + num2).toString()
          } else if (opperation[2] === '-') {
            calculatedNum = (num1 - num2).toString()
          }

          out = out.replace(opperation[0], calculatedNum)
        }
      }
      // Part 2

      if (depth !== 0) {
        return 'Invalid expression: missing one or more parentheses'
      }
      if (/^-?\d+\.?\d*$/.test(out) === false) { // tests if the output is a number using a regex
        return 'Invalid expression'
      }
      return out
    },
    makeTreeFromString: function (expString: string) {
      const opperators = {
        add: Symbol('add'),
        subtract: Symbol('subtract'),
        divide: Symbol('divide'),
        multiply: Symbol('multiply')
      }
    },
    calculateExpression: function (expString: string) {
      return expString
    },
    solve: function () {
      this.inputText = this.expressionInput
      this.solutionText = this.calculateExpression(this.expressionInput)
      this.showSolution = true
    }
  }
})
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.hidden {
  visibility: hidden;
}

.shown {
  visibility: visible;
}

h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
