<template>
  <div class="hello">
    <three :arrLoader="arrLoader" ref="child"></three>
    <div class="btn">
      <button id="btnA" @touchstart="textureLoader('a.jpg')">a.jpg</button>
      <button id="btnB" @touchstart="textureLoader('b.jpg')">b.jpg</button>
      <button id="btnC" @touchstart="textureLoader('c.jpg')">c.jpg</button>
      <button id="btnG" @touchstart="textureLoader('d.jpg')">d.jpg</button>
      <button id="btnD" @touchstart="modelLoader('file3.obj')">浴缸</button>
      <button id="btnE" @touchstart="modelLoader('file2.obj')">模特1</button>
      <button id="btnF" @touchstart="modelLoader('file1.obj')">模特2</button>
    </div>
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
          material: 'a.jpg',
          position: [100, 100, 100]
        }
      ]
    }
  },
  methods: {
    textureLoader (val) {
      this.arrLoader[0].material = val
      this.arrLoader[0].position = [2000, 200, 200]
      this.$refs.child.textureLoader(val)
    },
    modelLoader (val) {
      this.arrLoader[0].geomatry = val
      this.$refs.child.modelLoader(val)
      this.arrLoader[0].position = [300, 300, 300]
    }
  }
}
</script>

<style scoped>
*{
  border: 0;
  margin:0;
  padding:0;
}
.hello{
}
.btn{
  width: 265px;
  position: absolute;
  bottom: 0;
  right: 0;
}
.btn button{
  height: 50px;
}
</style>
