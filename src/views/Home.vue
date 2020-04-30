<template lang="pug">
  div(v-on:click="newRandomArray").slotmachine
    p.slots
      span {{luckyWords[0]}}
      span {{luckyWords[1]}}
      span {{luckyWords[2]}}
    //button(v-on:click="newRandomArray") Run
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'Home',
  data: function () {
    return {
      luckyWords: ['', '', ''],
      words: ['Moral', 'Interventions', 'Dark Patterns', 'Design', 'Possibility', 'Individual', 'Digital Wellness', 'Self–Help', 'Complexity ', 'World', 'Planet Earth ', 'Planet Earth ', 'Space', 'Religion', 'Religious', 'Spiritual', 'Schlaraffenland', 'Heaven', 'Art', 'Revolution', 'Voice', 'Commune', 'Progress', 'Humanity', 'Romantic', 'Relationship', 'Earthly Paradise', 'Paradise', 'Money', 'Commercialism', 'Capitalism', 'Socialism', 'New', 'Work', 'Freedom', 'Space', 'Faith', 'Harmony', 'A God', 'Cooperation', 'Whole', 'Souls', 'Spirituality', 'Nature', 'Sin', 'Living Standards', 'Optimism', 'Pessimism', 'Feminism', 'Critics', 'Extinction', 'Sustainability ', 'Human Conditions', 'Human Nature', 'Suffering', 'Death', 'Myths', 'Garden Eden', 'Happily', 'Critical Times ', 'Cultures', 'Countries', 'Societies', 'Memory', 'History', 'Past ', 'Future', 'Primitive', 'Dream', 'Premature Truth', 'Keep Shining ', 'Social Distancing ', 'Speculations', 'Epidemic ', 'Frequent ', 'Principles', 'Eternal Life ', 'Death Itself', 'Beginning', 'End ', 'Ridiculous', 'Impossible', 'Possible', 'Desire', 'Urge', 'Imagination', 'Technology', 'Infastructure', 'Attention', 'Empathy', 'Addictive', 'Unadressed', 'Adressed', 'Real–World', 'VR', 'Perfect User', 'Protest', 'Health', 'Creative', 'Medicine', 'Cure', 'Meditation', 'Balance', 'Social', 'Media', 'Hunger', 'Water', 'Crisis', 'Global', 'Local ', 'Market', 'Food', 'Feed', 'Family ', 'Single', 'Friendly', 'Radical', 'The Planet as', 'Night Vision', 'Puzzelmaker', 'Flying Cars', 'Ego', 'Killer', 'Heaven', 'Sunrise', 'Turn On', 'Tune In', 'Drop Out', 'Politics', 'Anti', 'Pro', 'Movement', 'Culture', 'Art', 'Counterculture', 'Climate', 'cool', 'high quality', 'Communalism', 'sharing Economy', 'democratic', 'Tool', 'Learning', 'Prototyping', 'Future', 'Publishing', 'Fresh', 'Biosphere', 'The Computer as', 'Cultivating', 'Spaceship Earth', 'Software', 'Mother', 'Father', 'Friends', 'Gene', 'Cybernetics', 'Independent', 'Low Cost', 'Offline', 'Moment Village', 'flying Balloons', 'Instant', 'Librarys', 'Bootleg', 'Architecture', 'Architects', 'do-it-yourself', 'Geodesic', 'Geodesic Dome', 'Desks', 'Movable', 'Revolution', 'Withput', 'Designer', 'Design', 'Cabin', 'Money', 'in the Desert', 'Alternative', 'Organs', 'Media', 'Internet', 'Milk & Honey', 'Scrolling']
    }
  },
  components: {
    HelloWorld
  },
  mounted () {
    this.newRandomArray()
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
    getRandomElement: function (myArray) {
      return myArray[Math.floor(Math.random() * myArray.length)]
    },
    newRandomArray: function () {
      this.luckyWords = this.luckyWords.map(word => this.getRandomElement(this.words))
      this.copyToClipboard(this.luckyWords.join(' '))
    }
  }
}
</script>

<style lang="stylus">
  .slotmachine
    width 80vw
    margin 1rem auto
    margin-top 30vh
    font-size 4rem
    cursor:  url('../assets/coin.png') 60 60, progress
  .slots
    width 100%
    display flex
    flex-direction row
    justify-content space-between
  @media screen and (max-width: 800px)
    .slotmachine
      margin-top 10vh
    .slots
      flex-direction column

</style>
