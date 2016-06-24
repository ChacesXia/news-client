<template>
  <div id="islider"></div>
</template>

<script>
import ISlider from '../assets/js/iSlider'
import buildIsliderAnimation from '../assets/js/iSlider.animate'
buildIsliderAnimation(ISlider)
export default {
  data () {
    return {
      slider: null
    }
  },
  ready () {
    var list = [
      {content: require('src/assets/img/1.jpg')},
      {content: require('src/assets/img/2.jpg')},
      {content: require('src/assets/img/3.jpg')},
      {content: require('src/assets/img/4.jpg')},
      {content: require('src/assets/img/5.jpg')}
    ]
    let self = this
    this.slider = new ISlider({
      dom: self.$el,
      data: list,
      isVertical: false,
      isLooping: true,
      isDebug: false,
      isAutoplay: true,
      animateType: 'rotate',
      animateTime: '1500',
      animateEasing: 'ease-in-out',
      isOverspread: true,
      fillSeam: true
    })
    // this.$http.get('banner.json')
    // .then(({data: {code, message, data}})=>{
    //   let imgs = []
    //   for (let i = 0 ; i < data.length; i++) {
    //     imgs.push(data[i])
    //   }
    //   let self = this
    //   console.log(self)
    //   this.slider = new ISlider({
    //     dom: self.$el,
    //     data: imgs,
    //     isVertical: false,
    //     isLooping: true,
    //     isDebug: true,
    //     isAutoplay: true,
    //     animateType: '3d'
    //   })
    // })
  }
}
</script>

<style lang="sass">
#islider {
  height: 200px;
  width: 100%;
  overflow: hidden;
  position: relative;

  ul {
    list-style: none;
    margin: 0;
    padding: 0;
    height: 100%;
    overflow: hidden;
  }

  li {
    position: absolute;
    margin: 0;
    padding: 0;
    height: 100%;
    overflow: hidden;
    display: -webkit-box;
    -webkit-box-pack: start;
    list-style: none;

    img {
      width: 100%;
      height: auto;
    }
  }
}
</style>
