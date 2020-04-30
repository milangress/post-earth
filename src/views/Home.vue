<template lang="pug">
  div.slotmachine
    div.slots
      p {{luckyWords[0]}}
      p {{luckyWords[1]}}
      p {{luckyWords[2]}}
    button(v-on:click="newRandomArray") Run
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'Home',
  data: function () {
    return {
      luckyWords: ['', '', ''],
      words: ['Moral', 'Interventions', 'Dark Patterns', 'Design', 'Possibility', 'Individual', 'Digital Wellness', 'Self–Help', 'Complexity ', 'World', 'Planet Earth ', 'Planet Earth ', 'Space', 'Religion', 'Religious', 'Spiritual', 'Schlaraffenland', 'Heaven', 'Art', 'Revolution', 'Voice', 'Commune', 'Progress', 'Humanity', 'Romantic', 'Relationship', 'Earthly Paradise', 'Paradise', 'Money', 'Commercialism', 'Capitalism', 'Socialism', 'New', 'Work', 'Freedom', 'Space', 'Faith', 'Harmony', 'A God', 'Cooperation', 'Whole', 'Souls', 'Spirituality', 'Nature', 'Sin', 'Living Standards', 'Optimism', 'Pessimism', 'Feminism', 'Critics', 'Extinction', 'Sustainability ', 'Human Conditions', 'Human Nature', 'Suffering', 'Death', 'Myths', 'Garden Eden', 'Happily', 'Critical Times ', 'Cultures', 'Countries', 'Societies', 'Memory', 'History', 'Past ', 'Future', 'Primitive', 'Dream', 'Premature Truth', 'Keep Shining ', 'Social Distancing ', 'Speculations', 'Epidemic ', 'Frequent ', 'Principles', 'Eternal Life ', 'Death Itself', 'Beginning', 'End ', 'Ridiculous', 'Impossible', 'Possible', 'Desire', 'Urge', 'Imagination', 'Technology', 'Infastructure', 'Attention', 'Empathy', 'Addictive', 'Unadressed', 'Adressed', 'Real–World', 'VR', 'Perfect User', 'Protest', 'Health', 'Creative', 'Medicine', 'Cure', 'Meditation', 'Balance', 'Social', 'Media', 'Hunger', 'Water', 'Crisis', 'Global', 'Local ', 'Market', 'Food', 'Feed', 'Family ', 'Single', 'Friendly']
    }
  },
  components: {
    HelloWorld
  },
  mounted () {
    this.newRandomArray()
  },
  methods: {
    getRandomElement: function (myArray) {
      return myArray[Math.floor(Math.random() * myArray.length)]
    },
    newRandomArray: function () {
      console.log(this.getRandomElement(this.words))
      this.luckyWords = this.luckyWords.map(word => {
        return this.getRandomElement(this.words)
      })
    }
  }
}
</script>

<style lang="stylus">
  .slotmachine
    width 80vw
    margin 1rem auto
    font-size 2rem
  .slots
    width 100%
    display flex
    flex-direction row
    justify-content space-between
</style>
