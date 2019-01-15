<template>
  <div class="hello">
    <div class="btnSel">
      <!-- <button id="btnA" @touchstart="textureLoader('a.jpg')">a.jpg</button>
      <button id="btnB" @touchstart="textureLoader('b.jpg')">b.jpg</button>
      <button id="btnC" @touchstart="textureLoader('c.jpg')">c.jpg</button>
      <button id="btnG" @touchstart="textureLoader('d.jpg')">d.jpg</button>
      <button id="btnD" @touchstart="modelLoader('file3.obj','b.jpg')">浴缸</button>
      <button id="btnE" @touchstart="modelLoader('file2.obj','b.jpg')">模特1</button>
      <button id="btnF" @touchstart="modelLoader('file1.obj','b.jpg')">模特2</button> -->
    </div>
  </div>
</template>

<script>
import * as THREE from 'three'
// import OrbitControls from 'three-orbitcontrols'
import OrbitControls from '../assets/js/myControls.js'
import {OBJLoader} from 'three-obj-mtl-loader'
import Stats from 'stats-js'
export default {
  props: ['arrLoader'],
  data () {
    return {
      scene: null,
      camera: null,
      renderer: null,
      container: null,
      material: null,
      loaderArr: null,
      stats: null,
      imgt: 'a.jpg'
    }
  },
  methods: {
    initRender () {
      this.renderer = new THREE.WebGLRenderer({
        antialias: true // 是否抗锯齿
      })
      this.renderer.setClearColor(0x000000, 1) // 设置颜色及透明度
      this.renderer.setSize(window.innerWidth, window.innerHeight)
      this.container = document.createElement('div')
      document.body.appendChild(this.container)
      this.container.appendChild(this.renderer.domElement)
    },
    initCamera () {
      this.camera = new THREE.PerspectiveCamera(60, window.innerWidth / window.innerHeight, 0.01, 1e10)
      this.camera.position.set(0, 2000, 1000)
    },
    initScene () {
      this.scene = new THREE.Scene()
      this.scene.add(this.camera)
      var helper = new THREE.AxesHelper(50)
      this.scene.add(helper)
    },
    initModel () {
      this.arrLoader.map(val => {
        // console.log(val, val.geomatry, val.material)
        this.imgt = val.material
        this.modelLoader(val.geomatry)
      })
      // this.modelLoader('file1.obj')
    },
    modelLoader (geomatry, material) {
      // console.log(geomatry, material)
      // console.log(222222222222, this.arrLoader)
      if (this.loaderArr) {
        for (let i = 0; i < this.loaderArr.length - 1; i++) {
          this.loaderArr[i].geometry.dispose()// 清理内存
          this.loaderArr[i].material.dispose()
        }
        this.loaderArr = null
      }
      this.scene.remove.apply(this.scene, this.scene.children)
      new OBJLoader().load('/static/obj/' + geomatry, obj => {
        this.loaderArr = obj.children
        this.textureLoader(this.imgt)
        this.scene.add(obj)
      })
    },
    textureLoader (img) {
      // console.log(111111111111, this.arrLoader)
      this.imgt = img
      var textureLoader = new THREE.TextureLoader()
      textureLoader.load(require('../assets/resources/' + img), texture => {
        texture.wrapS = texture.wrapT = THREE.RepeatWrapping
        texture.minFilter = THREE.NearestFilter
        texture.matrixAutoUpdate = false // 法向更新
        texture.needsUpdate = true
        this.material = new THREE.MeshBasicMaterial({
          map: texture
        })
        this.loaderArr.forEach(child => {
          child.material = this.material
          child.geometry.computeFaceNormals()
          child.geometry.computeVertexNormals()
        })
      })
    },
    initControls () {
      // controls = new THREE.TrackballControls(this.camera)
      this.controls = new OrbitControls(this.camera)
      this.controls.rotateSpeed = 1 // 旋转速度
      this.controls.zoomSpeed = 1 // 缩放/移动速度
      this.controls.panSpeed = 1 // 平移速度
      // console.log(this.controls)
    },
    initStats () {
      this.stats = new Stats()
      this.stats.domElement.style.position = 'absolute'
      this.stats.domElement.style.top = '0px'
      this.container.appendChild(this.stats.domElement)
    },
    animate () {
      this.renderer.render(this.scene, this.camera)
      this.stats.update()
      // console.log(this.controls)
      // this.controls.update();
      this.controls.update()
      requestAnimationFrame(this.animate)
    },

    draw () {
      this.initRender()
      this.initCamera()
      this.initScene()
      this.initModel()
      this.initControls()
      this.initStats()
      this.animate()
      // window.addEventListener('resize', onWindowResize, false) // 窗口调整
    }
  },
  created () {
    this.draw()
  }
}
</script>

<style scoped>
</style>
