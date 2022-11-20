<template>
  <div>
    <div id="bm" class="bm" ref="aaa"></div>
  </div>
</template>

<script>
import lottie from 'lottie-web'
console.log('lottie :>> ', lottie);
import bgRipple from '@/assets/json/bg_ripple.json'
console.log('bgRipple :>> ', bgRipple);

  export default {
    data() {
      return {
        text: 'aaa',
      };
    },
    beforeMount() {
      
    },
    mounted() {
      this.aaa()
    },
    methods: {
      aaa() {
        const animation = lottie.loadAnimation({
          container: document.getElementById('bm'),
          renderer: 'svg', 
          loop: false, 
          autoplay: true,
          path: 'https://labs.nearpod.com/bodymovin/demo/markus/halloween/markus.json',
        })
        animation.addEventListener('complete', function() {
          animation.destroy()
          document.getElementById('bm').style.background = '#ff0000'
        })
      }
    }
  }
</script>

<style scoped>
.bm {
  width: 300px;
  height: 300px;
}
</style>