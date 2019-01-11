<template>
  <div class="hello">
      <button id="btnA" @click="textureLoader('a.jpg')">a.jpg</button>
      <button id="btnB" @click="textureLoader('b.jpg')">b.jpg</button>
      <button id="btnC" @click="textureLoader('c.jpg')">c.jpg</button>
      <button id="btnG" @click="textureLoader('d.jpg')">d.jpg</button>
      <button id="btnD" @click="modelLoader('file3.obj')">浴缸</button>
      <button id="btnE" @click="modelLoader('file2.obj')">模特1</button>
      <button id="btnF" @click="modelLoader('file1.obj')">模特2</button>
<three :arrLoader="arrLoader" ref="child"></three>
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
          material: 'a.jpg'
        }
      ]
    }
  },
  methods: {
    textureLoader (val) {
      // this.matrial = val
      this.arrLoader[0].matrial = val
      console.log(1111100000, this.arrLoader)
      // this.$emit('textureLoader', val)
      this.$refs.child.textureLoader(val)
    },
    modelLoader (val) {
      this.arrLoader[0].geomatry = val
      console.log(222222211111111, this.arrLoader)
      this.$refs.child.modelLoader(val, this.arrLoader[0].material)
      // this.$emit('modelLoader', val)
    }
  }
}
</script>

<style scoped>
</style>
