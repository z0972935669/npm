<template>
    <div>
        {{msg}}
        <br>
        {{options}}
        <div id="bm" v-if="eee"></div>
        <div id="bm1" v-if="fff"></div>
        <div id="bm2" v-if="ggg"></div>
    </div>
</template>

<script>
    import lottie from 'lottie-web'

    export default {
        props: ['msg', 'options'],
        data() {
            return {
                eee: false,
                fff: false,
                ggg: false,
                text1: 'aaa',
                text2: 'bbb',
                text3: 'ccc',
            }
        },
        updated() {
            if (this.msg === 3 && (this.options.step.percent === 100 && this.options.step.checked === true)) {
                this.fff = false;
                this.ggg = false;
                this.cccc()
            } else if (this.msg === 4 && (this.options.heart.percent === 100 && this.options.heart.checked === true)) {
                this.eee = false;
                this.ggg = false;
                this.dddd()
            } else if (this.msg === 5 && (this.options.sleep.percent === 100 && this.options.sleep.checked === true)) {
                this.eee = false;
                this.fff = false;
                this.eeee()
            }
        },
        methods: {
            cccc() {
                this.eee = true;
                lottie.loadAnimation({
                    container: document.getElementById('bm'),
                    renderer: 'svg', 
                    name: this.text1, 
                    loop: false,
                    autoplay: true,
                    path: 'https://labs.nearpod.com/bodymovin/demo/markus/halloween/markus.json',
                    rendererSettings: {
                        className: 'bm',
                    }
                })
                lottie.destroy(this.text2);
                lottie.destroy(this.text3);
            },
            dddd() {
                this.fff = true;
                lottie.loadAnimation({
                    container: document.getElementById('bm1'),
                    renderer: 'svg', 
                    name: this.text2, 
                    loop: false, 
                    autoplay: true,
                    path: 'https://labs.nearpod.com/bodymovin/demo/markus/isometric/markus2.json',
                    rendererSettings: {
                        className: 'bm1',
                    }
                })
                lottie.destroy(this.text1);
                lottie.destroy(this.text3);
            },
            eeee() {
                this.ggg = true;
                lottie.loadAnimation({
                    container: document.getElementById('bm2'),
                    renderer: 'svg', 
                    name: this.text3, 
                    loop: false, 
                    autoplay: true,
                    path: 'https://labs.nearpod.com/bodymovin/demo/mattjedrzejewski/data.json',
                    rendererSettings: {
                        className: 'bm2',
                    }
                })
                lottie.destroy(this.text1);
                lottie.destroy(this.text2);
            },
        }
    }
</script>

<style scoped>
.bm svg {
    display: none;
}
.bm1 svg {
    display: none;
}
</style>