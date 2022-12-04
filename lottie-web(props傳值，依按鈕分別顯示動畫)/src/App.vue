<template>
  <div>
    <button @click="aaa">aaa</button>
    <button @click="bbb">bbb</button>
    <button @click="ccc">ccc</button>
    <About :msg="text" :options="dataObj" :firstLoading="firstLoading" />
  </div>
</template>

<script>
import About from '@/components/About'

  export default {
    data() {
      return {
        firstLoading: 4,
        text: 'aaa',
        dataObj: {
          step: {
            percent: 100,
            checked: true,
          },
          heart: {
            percent: 100,
            checked: true,
          },
          sleep: {
            percent: 100,
            checked: true,
          },
        }
      };
    },
    components: {
      About
    },  
    beforeMount() {
      
    },
    mounted() {
    },
    methods: {
      aaa() {
        this.text = 3
      },
      bbb() {
        this.text = 4
      },
      ccc() {
        this.text = 5
      },
    }
  }
</script>

<style scoped>
.bm {
  width: 300px;
  height: 300px;
}
</style>