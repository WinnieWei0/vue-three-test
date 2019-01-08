<template>
  <div class="hello">
      <button id="btnA" @click="textureLoader('a.jpg')">a.jpg</button>
      <button id="btnB" @click="textureLoader('b.jpg')">b.jpg</button>
      <button id="btnC" @click="textureLoader('c.jpg')">c.jpg</button>
      <button id="btnG" @click="textureLoader('d.jpg')">d.jpg</button>
      <button id="btnD" @click="modelLoader('file3.obj')">浴缸</button>
      <button id="btnE" @click="modelLoader('file2.obj')">模特1</button>
      <button id="btnF" @click="modelLoader('file1.obj')">模特2</button>
<three :arrLoader="arrLoader"></three>
  </div>
</template>

<script>
import Three from './Three'
export default {
  components: {Three},
  name: 'HelloWorld',
  data () {
    return {
      arrLoader: [
        {
          geomatry: 'file1.obj',
          matrial: 'a.jpg'
        }
      ]
      // geomatry: null,
      // matrial: null
    }
  },
  methods: {
    textureLoader (val) {
      // this.matrial = val
      this.arrLoader[0].matrial = val
    },
    modelLoader (val) {
      this.arrLoader[0].geomatry = val
    }
  }
}
</script>

<style scoped>
</style>
