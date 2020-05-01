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
      chosenWords: [],
      words: ['Moral', 'Interventions', 'Dark Patterns', 'Design', 'Possibility', 'Individual', 'Digital Wellness', 'Self–Help', 'Complexity ', 'World', 'Planet Earth ', 'Planet Earth ', 'Space', 'Religion', 'Religious', 'Spiritual', 'Schlaraffenland', 'Heaven', 'Art', 'Revolution', 'Voice', 'Commune', 'Progress', 'Humanity', 'Romantic', 'Relationship', 'Earthly Paradise', 'Paradise', 'Money', 'Commercialism', 'Capitalism', 'Socialism', 'New', 'Work', 'Freedom', 'Space', 'Faith', 'Harmony', 'A God', 'Cooperation', 'Whole', 'Souls', 'Spirituality', 'Nature', 'Sin', 'Living Standards', 'Optimism', 'Pessimism', 'Feminism', 'Critics', 'Extinction', 'Sustainability ', 'Human Conditions', 'Human Nature', 'Suffering', 'Death', 'Myths', 'Garden Eden', 'Happily', 'Critical Times ', 'Cultures', 'Countries', 'Societies', 'Memory', 'History', 'Past ', 'Future', 'Primitive', 'Dream', 'Premature Truth', 'Keep Shining ', 'Social Distancing ', 'Speculations', 'Epidemic ', 'Frequent ', 'Principles', 'Eternal Life ', 'Death Itself', 'Beginning', 'End ', 'Ridiculous', 'Impossible', 'Possible', 'Desire', 'Urge', 'Imagination', 'Technology', 'Infastructure', 'Attention', 'Empathy', 'Addictive', 'Unadressed', 'Adressed', 'Real–World', 'VR', 'Perfect User', 'Protest', 'Health', 'Creative', 'Medicine', 'Cure', 'Meditation', 'Balance', 'Social', 'Media', 'Hunger', 'Water', 'Crisis', 'Global', 'Local ', 'Market', 'Food', 'Feed', 'Family ', 'Single', 'Friendly', 'Radical', 'The Planet as', 'Night Vision', 'Puzzelmaker', 'Flying Cars', 'Ego', 'Killer', 'Heaven', 'Sunrise', 'Turn On', 'Tune In', 'Drop Out', 'Politics', 'Anti', 'Pro', 'Movement', 'Culture', 'Art', 'Counterculture', 'Climate', 'cool', 'high quality', 'Communalism', 'sharing Economy', 'democratic', 'Tool', 'Learning', 'Prototyping', 'Future', 'Publishing', 'Fresh', 'Biosphere', 'The Computer as', 'Cultivating', 'Spaceship Earth', 'Software', 'Mother', 'Father', 'Friends', 'Gene', 'Cybernetics', 'Independent', 'Low Cost', 'Offline', 'Moment Village', 'flying Balloons', 'Instant', 'Librarys', 'Bootleg', 'Architecture', 'Architects', 'do-it-yourself', 'Geodesic', 'Geodesic Dome', 'Desks', 'Movable', 'Revolution', 'Withput', 'Designer', 'Design', 'Cabin', 'Money', 'in the Desert', 'Alternative', 'Organs', 'Media', 'Internet', 'Milk & Honey', 'Scrolling']
    }
  },
  components: {
    HelloWorld
  },
  mounted () {
    this.slots.map(slot => { slot.items = this.shuffleArray(this.words) })
  },
  methods: {
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
    height: 200px
    overflow-y: hidden

  .slot__wrap

  .slot__item
    margin-top: 20px
    height: 160px
    padding: 0 10px
    text-align: center
    background-color: white
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
