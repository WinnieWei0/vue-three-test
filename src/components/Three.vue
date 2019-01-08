<template>
  <div class="hello">
    <div class="btnSel">
      <button id="btnA" @click="textureLoader(loaderArr,'a.jpg')">a.jpg</button>
      <button id="btnB" @click="textureLoader(loaderArr,'b.jpg')">b.jpg</button>
      <button id="btnC" @click="textureLoader(loaderArr,'c.jpg')">c.jpg</button>
      <button id="btnG" @click="textureLoader(loaderArr,'d.jpg')">d.jpg</button>
      <button id="btnD" @click="modelLoader('file3.obj')">浴缸</button>
      <button id="btnE" @click="modelLoader('file2.obj')">模特1</button>
      <button id="btnF" @click="modelLoader('file1.obj')">模特2</button>
    </div>
  </div>
</template>

<script>
import * as THREE from 'three'
import OrbitControls from 'three-orbitcontrols'
import {OBJLoader} from 'three-obj-mtl-loader'
import Stats from 'stats-js'
export default {
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
    onWindowResize () {
      this.camera.aspect = window.innerWidth / window.innerHeight
      this.camera.updateProjectionMatrix() // 更新相机投影矩阵
      this.renderer.setSize(window.innerWidth, window.innerHeight)
    },

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
      this.camera.position.set(0, 40, 50)
    },
    initScene () {
      this.scene = new THREE.Scene()
      this.scene.add(this.camera)
      var helper = new THREE.AxesHelper(50)
      this.scene.add(helper)
    },
    initModel () {
      this.modelLoader('file1.obj')
    },
    initControls () {
      this.controls = new OrbitControls(this.camera)
      this.controls.rotateSpeed = 3 // 旋转速度
      this.controls.zoomSpeed = 3 // 缩放/移动速度
      this.controls.panSpeed = 1 // 平移速度
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
      this.controls.update()
      requestAnimationFrame(this.animate)
    },

    textureLoader (obj, img) {
      this.imgt = img
      var textureLoader = new THREE.TextureLoader()
      textureLoader.load(require('../assets/resources/' + img), (texture) => {
        texture.wrapS = texture.wrapT = THREE.RepeatWrapping
        texture.minFilter = THREE.NearestFilter
        texture.matrixAutoUpdate = false // 法向更新
        texture.needsUpdate = true
        this.material = new THREE.MeshBasicMaterial({
          map: texture
        })
        obj.forEach((child) => {
          child.material = this.material
          child.geometry.computeFaceNormals()
          child.geometry.computeVertexNormals()
        })
      })
    },
    modelLoader (file) {
      if (this.loaderArr) {
        for (let i = 0; i < this.loaderArr.length - 1; i++) {
          this.loaderArr[i].geometry.dispose()// 清理内存
          this.loaderArr[i].material.dispose()
        }
        this.loaderArr = null
      }
      this.scene.remove.apply(this.scene, this.scene.children)
      var objLoader = new OBJLoader()
      objLoader.load(require('../assets/obj/' + file), (obj) => {
        this.loaderArr = obj.children
        this.textureLoader(obj.children, this.imgt)
        this.scene.add(obj)
      })
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
    this.$nextTick(() => {
      this.draw()
    })
  }
}
</script>

<style scoped>
</style>
