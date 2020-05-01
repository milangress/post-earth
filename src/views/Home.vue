<template lang="pug">
  div(@click="start").slotmachine
      //button(@click='start') start
      .slot(v-for='slot in slots' ref='slots')
        //h2 {{ slot.title }}
        .slot__window
          .slot__wrap
            .slot__item(v-for='opt in slot.items') {{ opt }}
            .slot__item.slot__item--copy {{ slot.items[0] }}

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
        items: []
      }, {
        items: []
      }, {
        items: []
      }],
      opts: null,
      startedAt: null,
      chosenWords: []
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

  .slotmachine
    //width 80vw
    display flex
    justify-content space-between
    margin 1rem auto
    margin-top 30vh
    font-size 3.5rem
    cursor:  url('../assets/coin.png') 60 60, progress
  @media screen and (max-width: 800px)
    .slotmachine
      margin-top 0vh
      flex-direction column
    .slot
      margin 0.2em
</style>
