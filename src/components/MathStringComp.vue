<template>
  <div class="endSpace" @mousedown="handleClick(0, $event)"></div>
  <span v-for="(string, index) in contentsString.data" v-bind:id="index" v-bind:key="string.id" @mousedown="handleClick(index, $event)" class="container">
    <span v-if="string.tag === 'string'">
      <span v-for="(char, charIndex) in string.value" v-bind:id="charIndex" v-bind:key="char.id">
        <span class="text">{{ char }}</span>
      </span>

    </span>
    <span v-if="string.tag === 'sqrt'" class="container">
      <span v-if="index === cursorPosition && focused && cursorShown" class="cursor"></span>
      <span v-else class="space"></span>
      <span class="sqrtChar">√</span>
      <span class="container sqrtBody">
        <MathStringComp :contentsString="string.value"/>
      </span>
    </span>
  </span>
  <span v-if="contentsString.length === cursorPosition && focused && cursorShown" class="cursor"></span>
  <span v-else class="space"></span>
  <div class="endSpace" @mousedown="handleClick(contentsString.length-1, $event)"></div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import { MathString } from '@/components/MathInput.vue'

export default defineComponent({
  name: 'MathStringComp',
  props: {
    contentsString: MathString
  },
  data () {
    return {
    }
  }
})
</script>

<style scoped>
  span {
    display: inline-block;
    margin: 0px;
    border: 0px;
    padding: 0px;
  }

  .endSpace {
    flex: 1;
    display: inline-block;
    margin: 0px;
    border: 0px;
    padding: 0px;
    padding-top: 20px;
    padding-bottom: 20px;
    width: 1.5px;
    height: 1em;
  }

  .container {
    display: flex;
    height: 1em;
    padding: 0px;
    padding-top: 20px;
    padding-bottom: 20px;
    align-items: center;
    align-self: center;
  }

  .sqrtChar {
    margin-top: 5px;
    padding-top: 2px;
    font-size: 1em;
  }

  .sqrtBody {
    margin-top: 5px;
    padding: 0px;
    border-style: solid;
    border-width: 0px;
    border-top-width: 1px;
    padding-top: 2px;
    padding-bottom: 3px;
    border-top-color: black;
  }

  .text {
    min-width: 4.4px;
    height: 1em;
  }

  .cursor {
    width: 1.5px;
    background-color: black;
    height: 1em;
    max-height: 1em;
    margin: 0px;
    align-self: center;
  }

  .space {
    display: inline-block;
    width: 1.5px;
    height: 1em;
  }

  .extraButtons {
    margin-top: 10px;
  }
</style>
