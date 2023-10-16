<script>
  import lec3d from '@trickle/lec3d'
  import TWEEN from '@tweenjs/tween.js';
  import { onMount } from 'svelte';


  let element, textContent;
  const computerName = 'mac pro'
  // let computerModelName;
  const loadingTime = 3000;
  const { scene, camera, renderer, mountTo, addControls, getClickEventTargets} = lec3d.init({
      // axesHelperConfigs: {
      //   length: 1000
      // },
      rendererConfigs: {
        backgroundColor: 0xff0000
      },
  })
  const { mountTo: mountCss3dTo, createCss3dObject } = lec3d.initCss3d({scene, camera})
  const {mountTo: mountCss2dTo, createCss2dObject} = lec3d.initCss2d({scene, camera})
  renderer.setClearColor(0x000000, 1)
  camera.position.set(300, 400, 180)

  /** 加载模型 */
  const loadModels = (scene) => {
    lec3d.loadGLTF({ 
      modelPath: 'computers/scene.gltf', 
      options: {
        scale: 50,
      }, 
      callback: (gltf, model) => {
        scene.add(model)
    }})

    // TODO: 这个模型有点问题
    lec3d.loadGLTF({ 
      modelPath: 'macbook/scene.gltf', 
      options: {
        scale: 200,
        position: {
          x: 120
        }
      }, 
      callback: (gltf, model) => {
        model.name = computerName
        scene.add(model)
    }})

    lec3d.loadGLTF({ 
      modelPath: 'robot/scene.gltf', 
      options: {
        scale: 10,
        position: {
          x: 100
        },
        rotation: {
          y: '-120'
        }
      }, 
      callback: (gltf, model) => {
        scene.add(model)
    }})

    lec3d.loadGLTF({ 
      modelPath: 'cxh/scene.glb', 
      options: {
        scale: 10,
        position: {
          x: -82,
          y: 3,
          z: -20
        },
        rotation: {
        }
      }, 
      callback: (gltf, model) => {
        scene.add(model)
    }})

    lec3d.loadGLTF({ 
      modelPath: 'lucario/scene.gltf', 
      options: {
        scale: 20,
        position: {
          x: 15,
          y: 157,
          z: -145
        },
        rotation: {
        }
      }, 
      callback: (gltf, model) => {
        scene.add(model)
    }})

    lec3d.loadGLTF({ 
      modelPath: 'minecraft_torch/scene.gltf', 
      options: {
        scale: 8,
        position: {
          x: 100,
          y: 90,
          z: -58
        },
        rotation: {
          z: '-30',
          x: '45'
        }
      }, 
      callback: (gltf, model) => {
        scene.add(model)
    }})

    lec3d.loadGLTF({ 
      modelPath: 'minecraft_chest/scene.gltf', 
      options: {
        scale: 0.1,
        position: {
          x: 90,
          y: 107.5,
          z: -80
        },
        rotation: {
          y: '-130',
        }
      }, 
      callback: (gltf, model) => {
        scene.add(model)
        model.name = 'chest'
      }})
  }
  
  /** 平滑动画 */
  const updateAnimate = () => {
    TWEEN.update() 
    requestAnimationFrame(updateAnimate)
  }

  // 聚焦到屏幕中心，仅执行一次
  const focusFac = () => {
    let once = true
    return (callback) => {
      if (once) {
        new TWEEN.Tween(camera.position).to({
            x: 0,
            y: 10,
            z: 180,
          }, 
          
          loadingTime).onUpdate((obj)=> {
            // console.log(obj)
            camera.lookAt(0,30,0)
          }).onComplete(() => {
            once = false
            callback?.()
          }).start()
      }
    }
  } 
  const focus = focusFac()

    /** 点击事件处理函数 */
  const handleClick = (e) => {
    // 寻找指定name的模型
    const target = getClickEventTargets(e)?.[0]?.object
    let curTarget = target
    while(curTarget) {
      if (curTarget.name === computerName) {
        break
      }
      curTarget = curTarget.parent
    }
    // 点到了指定的电脑模型
    if(curTarget) {
      console.log('target', curTarget)
    }
  }



  const generateText = () => {
 
    // 动态控制这个内容
    textContent = document.createElement('iframe')
    textContent.style.border = 'none'
    textContent.width = '570'
    textContent.height = '404'
    textContent.scrolling = 'no'


    const css2dObject = createCss2dObject({content:textContent})
    css2dObject.position.set(-24, 60, -40)
    css2dObject.name = 'text'
    scene.add(css2dObject)
  }

  onMount(() => {
    // const controls = addControls()
    // controls.enabled = false
    updateAnimate()
    focus(()=>{
      // controls.enabled = true
      textContent.src = '/text.html'
    })

    window.addEventListener('click', handleClick)
    generateText()
    loadModels(scene)
    mountTo(element)
    setTimeout(() => {
      mountCss2dTo(element)
    }, 2000)
  })
</script>

<main>
  <div bind:this={element}></div>
</main>

<style>
</style>