<template>
  <div id="app">
    <input @change="fileSelected" type="file" accept=".csv">
    <template v-if="totals.length">
      <p v-for="total in totals" :key="total.name">{{ total.name }}: {{total.total}}</p>
    </template>
  </div>
</template>

<script>
import Papa from 'papaparse'

export default {
  name: 'app',
  data () {
    return {
      totals: []
    }
  },
  methods: {
    fileSelected () {
      const input = document.querySelector('input[type=file')
      Papa.parse(input.files[0], {
        complete: this.parseComplete,
        header: true
      })
    },

    parseComplete (results) {
      let products = []
      results.data.forEach((e) => {
        if (e['Product']) {
          products.push(e['Product'])
        }
      })
      products = Array.from(new Set(products)).sort()
      products.forEach((p) => {
        let total = 0
        results.data.forEach((i) => {
          if (i['Product'] === p) {
            total += 1
          }
        })
        this.totals.push({ name: p, total: total })
      })
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
