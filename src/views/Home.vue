<template lang="pug">
  div(@click="start" v-bind:style="bgc").wrapper
    .ImageOverlay
      img(src="@/assets/Plot-Slot-Maschine.png")
    .slotmachine
      .slot(v-for='slot in slots' ref='slots')
        //h2 {{ slot.title }}
        .slot__window
          .slot__wrap
            .slot__item(v-for='opt in slot.items') {{ opt }}
            .slot__item.slot__item--copy {{ slot.items[0] }}
    div.description
      p This is the Plot/Slot Machine – What does the future tell?
      p <span>Beginning</span><span>Libraries</span><span>Romantic</span>could be a the start of a wonderful nerdy love story and <span>Father</span><span>Desire</span><span>Alternative</span>sounds like there will be daddy issues.
      p But you can also question in with direction our society goes. <span>Climate</span><span>Garden Eden</span><span>Geodesic Dome</span>could envision a future where the rich seek refuge from a harsh climate under geodesic domes. While <span>Addictive</span><span>Scrolling</span><span>Politics</span>could mean that Instagram gets outlawed.
      p And you get to decide if a world in there everybody uses <span>Romantic</span><span>Counterculture</span><span>Money</span>and walks around with a sheet of LSD is a utopia or a dystopia.
      p You can use the plot slot machine for a conversation, as a jump of point for a creative process or as a definitive oracle and let it guide every aspect of your life. Either way we invite you to participate, to &nbsp;
        a(href="https://docs.google.com/spreadsheets/d/1UKl6-enr9-ih9rfZ52x9n66VXIELMA5lMkRJjDB4Bbo/edit#gid=0") add you own words to the list
        | &nbsp; and by that at least control your own future a little bit.

      p So don't forget – this is not <span>The Planet as</span><span>Low Cost</span><span>Culture</span>and always keep  <span>The</span><span>Optimism</span><span>Pessimism</span>
      p with Love
        br
        | Anna & Milan

</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'

const sheetURL = 'https://spreadsheets.google.com/feeds/cells/1UKl6-enr9-ih9rfZ52x9n66VXIELMA5lMkRJjDB4Bbo/1/public/full?alt=json'

const next = window.requestAnimationFrame ||
  window.webkitRequestAnimationFrame ||
  window.mozRequestAnimationFrame ||
  window.msRequestAnimationFrame ||
  window.oRequestAnimationFrame ||
  function (cb) { window.setTimeout(cb, 1000 / 60) }

export default {
  name: 'Home',
  data: function () {
    return {
      slots: [{
        items: ['Plot']
      }, {
        items: ['Slot']
      }, {
        items: ['Machine']
      }],
      opts: null,
      startedAt: null,
      chosenWords: [],
      bgc: {
        backgroundColor: 'rgb(255,255,255)'
      }
    }
  },
  components: {
    HelloWorld
  },
  mounted () {
    this.loadSheet()
    // this.slots.map(slot => { slot.items = [...slot.items, ...this.shuffleArray(this.words)] })
  },
  methods: {
    loadSheet: async function () {
      const sheetData = await fetch(sheetURL).then(response => response.json())
      const entries = sheetData.feed.entry.filter(entry => entry.gs$cell.row !== '1')
      const allSlots = entries.filter(entry => entry.gs$cell.col === '1').map(entry => entry.content.$t)
      const firstSlot = entries.filter(entry => entry.gs$cell.col === '2').map(entry => entry.content.$t)
      const secondSlot = entries.filter(entry => entry.gs$cell.col === '3').map(entry => entry.content.$t)
      const thirdSlot = entries.filter(entry => entry.gs$cell.col === '4').map(entry => entry.content.$t)
      this.slots = [{
        items: this.shuffleArray([...firstSlot, ...firstSlot, ...allSlots])
      }, {
        items: this.shuffleArray([...secondSlot, ...secondSlot, ...allSlots])
      }, {
        items: this.shuffleArray([...thirdSlot, ...thirdSlot, ...allSlots])
      }]
      console.log(entries)
      console.log(this.slots)
    },
    copyToClipboard: function (str) {
      const el = document.createElement('textarea')
      el.value = str
      document.body.appendChild(el)
      el.select()
      document.execCommand('copy')
      document.body.removeChild(el)
    },
    shuffleArray: function (arrParam) {
      const arr = arrParam.slice()
      let length = arr.length
      let temp
      let i
      while (length) {
        i = Math.floor(Math.random() * length--)
        temp = arr[length]
        arr[length] = arr[i]
        arr[i] = temp
      }
      return arr
    },
    getRandomElement: function (myArray) {
      return myArray[Math.floor(Math.random() * myArray.length)]
    },
    newColor: function () {
      this.bgc.backgroundColor = `rgb(${Math.random() * 255},${Math.random() * 255},${Math.random() * 255})`
    },
    start: function () {
      if (this.opts) {
        return
      }
      this.opts = this.slots.map((data, i) => {
        const slot = this.$refs.slots[i]
        const choice = Math.floor(Math.random() * data.items.length)
        console.log('choice', i, data.items[choice])
        this.chosenWords[i] = data.items[choice]
        const opts = {
          el: slot.querySelector('.slot__wrap'),
          finalPos: choice * 180,
          startOffset: 2000 + Math.random() * 500 + i * 500,
          height: data.items.length * 180,
          duration: 3000 + i * 700, // milliseconds
          isFinished: false
        }
        return opts
      })
      this.copyToClipboard(this.chosenWords.join('  '))
      next(this.animate)
    },
    animate: function (timestamp) {
      if (this.startedAt == null) {
        this.startedAt = timestamp
      }
      const timeDiff = timestamp - this.startedAt
      this.opts.forEach(opt => {
        if (opt.isFinished) {
          return
        }

        const timeRemaining = Math.max(opt.duration - timeDiff, 0)
        const power = 3
        const offset = (Math.pow(timeRemaining, power) / Math.pow(opt.duration, power)) * opt.startOffset

        // negative, such that slots move from top to bottom
        const pos = -1 * Math.floor((offset + opt.finalPos) % opt.height)

        opt.el.style.transform = 'translateY(' + pos + 'px)'
        this.newColor()

        if (timeDiff > opt.duration) {
          console.log('finished', opt, pos, opt.finalPost)
          opt.isFinished = true
        }
      })
      if (this.opts.every(o => o.isFinished)) {
        this.opts = null
        this.startedAt = null
        console.log('finished')
      } else {
        next(this.animate)
      }
    }
  }
}
</script>

<style lang="stylus">
  .wrapper
    padding 1rem
    height 100%
    //transition 0.1s
  .slot
    float: left
    margin: 0.4em

  .slot__window
    background-color: black
    background: linear-gradient(45deg, rgba(0,0,0,0.22), rgba(255,255,255,0.25))
    border-radius 4px
    height: 200px
    overflow-y: hidden
    box-shadow: 12px 12px 16px 0 rgba(0, 0, 0, 0.25), -8px -8px 12px 0 rgba(255, 255, 255, 0.3)

  .slot__wrap

  .slot__item
    margin-top: 20px
    height: 160px
    padding: 0 10px
    text-align: center
    background-color: white
    background: linear-gradient(-185deg, rgba(255,255,255,0.82), rgba(255,255,255,0.25))
    border-radius 2px
    color: black
    line-height: 160px

  .slot__item--copy

  .description
    max-width 75ch
    text-align left
    line-height 1.45
    margin 10rem auto
    font-size 2rem
  .description span
    //background linear-gradient(-185deg, rgba(255,255,255,0.82), rgba(255,255,255,0.25))
    border 1px solid black
    border-radius 0.2em
    padding 0.01em 0.15em
    margin 0 0.15em
    white-space:nowrap;
  a:visited
    color black
  .footer
    margin-top 10rem
  .slotmachine
    //width 80vw
    display flex
    justify-content space-between
    margin 1rem auto
    margin-top 30vh
    font-size 3.5rem
    //cursor:  url('../assets/coin-turn.png') 60 60, progress
  .ImageOverlay
    width 100vw
    max-height 100vh
    position fixed
    z-index 100
    margin 0 auto
    animation linear fade 8s
    opacity 0
    pointer-events: none
  .ImageOverlay img
    max-height 90vh
  @keyframes fade {
    0% {
      opacity 0
    }
    10% {
      opacity 1
    }
    100% {
      opacity 0
    }
  }

  @media screen and (max-width: 800px)
    .slotmachine
      margin-top 0vh
      flex-direction column
    .slot
      margin 0.2em
</style>
