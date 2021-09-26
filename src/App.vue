<template>
  <header id="header">
    <h1>Roman Numeral Converter</h1>
    <label>
      <input type="checkbox" v-model="enableViniculum" />
      Enable Viniculum Notation
    </label>
  </header>

  <textarea ref="inputEl" id="input" v-model="input" placeholder="Enter a number here 🙂"></textarea>

  <div id="output">
    <span class="overline">{{ overLine }}</span>
    <span>{{ output }}</span>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'

// text input
const input = ref('1776')
const inputEl = ref()
onMounted(() => inputEl.value.focus())

// enable viniculum checkbox value
const enableViniculum = ref(false)

/**
 * Roman numeral converter
 * @param {string} num - the number to convert
 * 
 * @returns {string}
 */
function convertToRoman(num) {
  if (num == 0 || isNaN(parseInt(num))) return 'nulla'

  let roman = ''

  while (num > 0) {
    if (num >= 1000) {
      roman += 'M'
      num -= 1000
    } else if (num >= 900) {
      roman += 'CM'
      num -= 900
    } else if (num >= 500) {
      roman += 'D'
      num -= 500
    } else if (num >= 400) {
      roman += 'CD'
      num -= 400
    } else if (num >= 100) {
      roman += 'C'
      num -= 100
    } else if (num >= 90) {
      roman += 'XC'
      num -= 90
    } else if (num >= 50) {
      roman += 'L'
      num -= 50
    } else if (num >= 40) {
      roman += 'XL'
      num -= 40
    } else if (num >= 10) {
      roman += 'X'
      num -= 10
    } else if (num >= 9) {
      roman += 'IX'
      num -= 9
    } else if (num >= 5) {
      roman += 'V'
      num -= 5
    } else if (num >= 4) {
      roman += 'IV'
      num -= 4
    } else if (num >= 1) {
      roman += 'I'
      num -= 1
    }
  }

  return roman
}

// depending on the value of enableViniculum,
// return the normal output, or remove the M's
const output = computed(() => {
  let num = convertToRoman(input.value)

  if (enableViniculum.value) {
    num = num.replace(/M/g, '')
  }

  return num
})

// if viniculum is enabled, return the overline
// otherwise, return an empty string
const overLine = computed(() => {
  if (enableViniculum.value) {
    const roman = convertToRoman(input.value)

    // count the occurrences of M in roman
    let count = (roman.match(/M/g) || []).length

    if (count) return convertToRoman(count)
  }

  return ''
})
</script>

<style>
:root {
  --background: rgba(255, 255, 255, 0.75);
  --border: 2px solid rgba(255, 255, 255, 0.514);
  --color: #000;
  --box-shadow: 0 5px 10px rgba(0, 0, 0, 0.185);
  --gradient: linear-gradient(125deg, rgb(212, 38, 111), orange, tomato);
}

@media (prefers-color-scheme: dark) {
  :root {
    --background: rgba(0, 0, 0, 0.8);
    --border: 2px solid rgba(0, 0, 0, 0.514);
    --color: #fff;
    --box-shadow: 0 5px 10px rgba(255, 255, 255, 0.185);
    --gradient: linear-gradient(
      125deg,
      rgb(141, 23, 72),
      rgb(185, 121, 0),
      rgb(161, 61, 43)
    );
  }
}

html,
body {
  padding: 0;
  margin: 0;
  height: 100%;
  background: var(--gradient);
  background-size: contain;
}

#app {
  font-family: sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: grid;
  gap: 0.5rem;
  padding: 0.5rem;
  box-sizing: border-box;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: auto 1fr;
  height: 100%;
}

#app > * {
  background: var(--background);
  padding: 1rem;
  color: var(--color);
  border-radius: 4px;
  border: var(--border);
  box-shadow: var(--box-shadow);
}

#header {
  grid-column: 1 / -1;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h1 {
  margin: 0.5rem;
}

label {
  user-select: none;
  cursor: pointer;
}

#input {
  resize: none;
  outline: none;
  font-size: 30px;
}

#output {
  font-size: 30px;
  word-wrap: break-word;
  min-width: 0;
  font-family: "Times New Roman", Times, serif;
}

.overline {
  text-decoration: overline;
}

/* when the screen is below a certain size, change #app grid template columns to 1fr */
@media screen and (max-width: 600px) {
  #app {
    grid-template-columns: 1fr;
    grid-template-rows: auto auto 1fr;
  }

  #header {
    flex-direction: column;
  }
}
</style>
