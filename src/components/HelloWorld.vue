<template>
  <div class="hello">
    <h3>Clear text</h3>
      <div>

      <textarea class="tx" v-model.trim="svg"></textarea>
      </div>
      <button @click="pack">Pack</button>
      <div>
      <h3>Gzip data(hex)</h3>

      <textarea class="tx" v-model="hex"></textarea>
      </div>
      <button @click="unpack">Unpack</button>
      <div>
      <h3>Verification</h3>

      <textarea class="tx" :class="{ok : isOk, err: !isOk}" readonly v-model="verify"></textarea>
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
  computed: {
    isOk () {
      return this.svg === this.verify
    }
  },
  methods: {
    pack () {
      let packed = pako.gzip(this.svg) // Uint8array
      let hex = packed.reduce((a, v) => a + ('0' + v.toString(16)).slice(-2), '')
      this.hex = '0x' + hex
      const bytes = new Uint8Array(hex.match(/.{1,2}/g).map(byte => parseInt(byte, 16)))
      const dec = new TextDecoder('utf-8')
      this.verify = dec.decode(pako.ungzip(bytes))
    },
    unpack () {
      let packed = this.hex
      if (packed.startsWith('0x')) packed = packed.substr(2)
      const bytes = new Uint8Array(packed.match(/.{1,2}/g).map(byte => parseInt(byte, 16)))
      const dec = new TextDecoder('utf-8')
      this.svg = dec.decode(pako.ungzip(bytes))
    }
  }
}
</script>

<style scoped>
.tx {
  width: 600px;
  max-width: 100%;
  height: 200px;
}
.ok {
    color: #008000;
}
.err {
    color: #a00101;
}
</style>
