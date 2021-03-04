<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
      Clear:
      <div>

      <textarea class="tx" v-model.trim="svg"></textarea>
      </div>
      <button @click="pack">Pack</button>
      <div>

      <textarea class="tx" readonly v-model="hex"></textarea>
      </div>
      <div>

      <textarea class="tx" readonly v-model="verify"></textarea>
      </div>

  </div>
</template>

<script>
import pako from 'pako'
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data () {
    return {
      svg : '',
      hex : '',
      verify: ''
    }
  },
  methods: {
    pack () {
      let packed = pako.gzip(this.svg) // Uint8array
      this.hex = packed.reduce((a, v) => a + ('0' + v.toString(16)).slice(-2), '0x')
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.tx {
  width: 600px;
  max-width: 100%;
  height: 200px;
}
</style>
