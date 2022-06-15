<script setup lang="ts">
import { ref, watch } from 'vue'

const radixs = ref({
  Dec: 10,
  Bin: 2,
  Hex: 16
})
const activeRadix = ref<'Dec' | 'Bin' | 'Hex'>('Dec')

const inputValue = ref<string>('')

const bin = ref<string | number>(0)
const hex = ref<string | number>(0)
const dec = ref<string | number>(0)

watch(inputValue, () => {
  bin.value = convert(
    inputValue.value ?? 0,
    radixs.value[activeRadix.value],
    2
  )
  hex.value = convert(
    inputValue.value ?? 0,
    radixs.value[activeRadix.value],
    16
  )
  dec.value = convert(
    inputValue.value ?? 0,
    radixs.value[activeRadix.value],
    10
  )
})

function convert(number: number | string, from: number, to: number) {
  console.log(number)
  if (from === 16) {
    number = parseInt('' + number, 16)
  }
  console.log(number)

  let result: string | number = parseInt('' + number, from)
  if (isNaN(result)) return result

  result = result.toString(to)

  if (to === 10) return ~~result
  return result
}
</script>

<template>
  <h1>Radix Calculator</h1>
  <div mx-auto p8 w-400px b-6 b="dashed gray-300" rounded-xl flex="~ col">
    <div
      flex
      justify-center
      items-center
      text-center
      text-xl
      font-bold
      text-gray-300
      cursor-pointer
    >
      <div v-for="(i, radix) in radixs" :key="radix" mx-4>
        <div
          :class="{ 'active-radix': activeRadix === radix }"
          @click="activeRadix = radix"
        >
          {{ radix }}
        </div>
      </div>
    </div>
    <div mx-auto my-4>
      <input block type="text" v-model="inputValue" />
    </div>
    <div p3 flex justify-center items-center>
      <div mx-4 text-xl font-bold>Bin:</div>
      <div>{{ bin }}</div>
    </div>
    <div p3 flex justify-center items-center>
      <div mx-4 text-xl font-bold>Dec:</div>
      <div>{{ dec }}</div>
    </div>
    <div p3 flex justify-center items-center>
      <div mx-4 text-xl font-bold>Hex:</div>
      <div>{{ hex }}</div>
    </div>
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.active-radix {
  color: #000;
}
</style>
