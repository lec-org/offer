<script>
  import lec3d from '@trickle/lec3d'
  import TWEEN from '@tweenjs/tween.js';
  import { onMount } from 'svelte';

  let element;
  let computerModelName;
  const { scene, camera, renderer, mountTo, addControls, getClickEventTargets} = lec3d.init({
      axesHelperConfigs: {
        length: 1000
      },
      rendererConfigs: {
        backgroundColor: 0xff0000
      },
  })
  
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

    lec3d.loadGLTF({ 
      modelPath: 'old_computer/scene.gltf', 
      options: {
        scale: 30,
      }, 
      callback: (gltf, model) => {
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
        computerModelName = model.name
    }})
  }
  
  /** 平滑动画 */
  const updateAnimate = () => {
    TWEEN.update() 
    requestAnimationFrame(updateAnimate)
  }

  /** 点击事件处理函数 */
  // const handleClick = (e) => {
  //   // 寻找指定name的模型
  //   const target = getClickEventTargets(e)?.[0]?.object
  //   let curTarget = target
  //   while(curTarget) {
  //     if (curTarget.name === computerModelName) {
  //       break
  //     }
  //     curTarget = curTarget.parent
  //   }
  //   // 点到了指定的电脑模型
  //   if(curTarget) {
  //     console.log('???', camera)
  //     new TWEEN.Tween(camera.position).to({
  //       x: 0,
  //       y: 80,
  //       z: 170,
  //     }, 1000).onUpdate((obj)=> {
  //       console.log(obj)
  //       camera.lookAt(0,0,0)
  //     }).start()
  //   }
  // }

  // 聚焦到屏幕中心，仅执行一次
  const focusFac = () => {
    let once = true
    return () => {
      if (once) {
        new TWEEN.Tween(camera.position).to({
            x: 0,
            y: 70,
            z: 180,
          }, 3000).onUpdate((obj)=> {
            console.log(obj)
            camera.lookAt(0,30,0)
          }).onComplete(() => {
            once = false
          }).start()
      }
    }
  } 

  const focus = focusFac()

  onMount(() => {
    renderer.setClearColor(0x000000,1)
    updateAnimate()
    focus()
    // window.addEventListener('click', handleClick)
    addControls({
      callback: (scene, camera) => {}
    })
    loadModels(scene)
    mountTo(element)
  })
</script>

<main>
  <div bind:this={element}></div>
</main>

<style>
</style>
