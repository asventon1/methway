<template>
  <div class="bigBox" @focusin="handleFocus" @focusout="handleUnfocus" tabindex=0>
    <MathStringComp :contents-string="contentsString"/>
  </div>
  <div class="extraButtons">
    <button>√</button>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import MathStringComp from '@/components/MathStringComp.vue'

export enum Opperator {
  add,
  subtract,
  multiple,
  divide,
  power,
  root,
}

export interface MathStringPieceString {
  tag: 'string'
  value: string
}

export interface MathStringPieceSqrt {
  tag: 'sqrt'
  value: MathString
}

export interface MathStringPieceDivide {
  tag: 'divide'
  value: [MathString, MathString]
}

export interface MathStringPiecePower {
  tag: 'power'
  value: [MathString, MathString]
}

export type MathStringPiece = MathStringPieceString | MathStringPieceSqrt | MathStringPieceDivide | MathStringPiecePower

export class MathString {
  data: MathStringPiece[]

  constructor (data: MathStringPiece[]) {
    this.data = data
  }
}

export default defineComponent({
  name: 'MathInput',
  props: {},
  components: {
    MathStringComp
  },
  data () {
    return {
      contents: {},
      contentsString: this.makeTestString(5),
      focused: false,
      bigBoxBorderColor: 'black',
      cursorPosition: 0,
      cursorShown: true,
      cursorInterval: 0
    }
  },
  mounted () {
    window.addEventListener('keypress', this.handleKeyPress)
    window.addEventListener('keydown', this.handleKeyDown)
    this.setCursorInterval()
  },
  unmounted () {
    window.removeEventListener('keypress', this.handleKeyPress)
    window.addEventListener('keydown', this.handleKeyDown)
  },
  methods: {
    makeTestString: function (count: number): MathString {
      if (count === 0) {
        return new MathString([{ tag: 'string', value: 'thing' }])
      }
      return new MathString([{ tag: 'string', value: 'test' }, { tag: 'sqrt', value: this.makeTestString(count - 1) }])
    },
    setCursorInterval: function () {
      this.cursorShown = true
      clearInterval(this.cursorInterval)
      this.cursorInterval = setInterval(
        () => { this.cursorShown = !this.cursorShown },
        500)
    },
    handleKeyPress: function (e: KeyboardEvent) {
      /*
      if (this.focused) {
        if (e.key === 'ArrowRight') {
          this.cursorPosition++
        } else if (e.key === 'ArrowLeft') {
          this.cursorPosition--
        }
        this.contentsString =
          this.contentsString.substring(0, this.cursorPosition) +
          String.fromCharCode(e.keyCode) +
          this.contentsString.substring(this.cursorPosition)
        this.cursorPosition++
      }
      */
    },
    handleKeyDown: function (e: KeyboardEvent) {
      /*
      if (this.focused) {
        if (e.key === 'ArrowRight') {
          if (this.cursorPosition < this.contentsString.length) {
            this.cursorPosition++
          }
          this.setCursorInterval()
        } else if (e.key === 'ArrowLeft') {
          if (this.cursorPosition > 0) {
            this.cursorPosition--
          }
          this.setCursorInterval()
        } else if (e.key === 'Backspace') {
          if (this.cursorPosition > 0) {
            this.cursorPosition--
            this.contentsString =
              this.contentsString.substring(0, this.cursorPosition) +
              this.contentsString.substring(this.cursorPosition + 1)
          }
        }
      }
      */
    },
    handleFocus: function () {
      this.focused = true
      this.bigBoxBorderColor = 'blue'
    },
    handleUnfocus: function () {
      this.focused = false
      this.bigBoxBorderColor = 'black'
    },
    handleClick: function (index: number, e: MouseEvent) {
      if (e.target !== null) {
        const selectedChar: HTMLElement = (e.target as HTMLElement).parentNode as HTMLElement

        const box = selectedChar.getBoundingClientRect()
        if (e.x - box.left < box.right - e.x) {
          this.cursorPosition = index
        } else {
          this.cursorPosition = index + 1
        }
        this.setCursorInterval()
      }
    }
  }
})

</script>

<style scoped>
  .bigBox {
    display: flex;
    align-items: flex-end;
    font-size: 20px;
    padding: 0px;
    min-height: 20px;
    border-style: solid;
    border-color: v-bind('bigBoxBorderColor');
    border-width: thick;
  }

</style>
