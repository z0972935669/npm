<template>
  <div>
    <div id="bm" class="bm" ref="aaa"></div>
  </div>
</template>

<script>
import lottie from 'lottie-web'
console.log('lottie :>> ', lottie);
import bgRipple from '@/assets/json/bg_ripple.json'
import bgRippleRoundness from '@/assets/json/bg_ripple_roundness.json'
import bgRippleRoundness1 from '@/assets/json/bg_ripple_roundness_1.json'
import highFive from '@/assets/json/high_five.json'
import loading from '@/assets/json/loading.json'
console.log('bgRipple :>> ', bgRipple);

  export default {
    data() {
      return {
        text: 'aaa',
      };
    },
    mounted() {
      this.aaa()
    },
    methods: {
      aaa() {
        const animation = lottie.loadAnimation({
          container: document.getElementById('bm'),
          renderer: 'canvas', 
          loop: false, 
          autoplay: true,
          // path: 'https://labs.nearpod.com/bodymovin/demo/markus/halloween/markus.json',
          animationData: bgRippleRoundness1,
          // animationData: highFive,
          // animationData: loading,
          rendererSettings: {
            className: 'canvas',
            id: 'canvas'
          }
        })

        
        
        
        animation.addEventListener('complete',() => {
          // animation.destroy()
          // document.getElementById('bm').style.background = '#ff0000'
        })
      },
    }
  }
</script>

<style scoped>
/* .bm {
  width: 300px;
  height: 300px;
} */
</style>