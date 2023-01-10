<template>
  <FavoriteColorCard
    :key = "favoriteColor"
    :name = "name"
    :byu-id = "byuId"
    :fav-color = "favoriteColor"
  />
</template>

<script lang="ts">
import { Component, Vue } from 'nuxt-property-decorator'
import FavoriteColorCard from '~/components/FavoriteColorCard.vue'

@Component({
  components: { FavoriteColorCard }
})
export default class Me extends Vue {
  name: string = this.$store.getters.username
  byuId: string = this.$store.state.user.byuId
  favoriteColor: string = ''

  options = [
    'blue',
    'red',
    'green',
    'yellow',
    'purple',
    'orange',
    'black',
    'pink',
    'indigo',
    'cyan',
    'teal',
    'lime',
    'amber',
    'brown',
    'grey'
  ]

  async mounted () {
    const response = await this.$axios.$get(`https://jtbt2-fav-color-dev.byu-oit-fullstack-trn.amazon.byu.edu/${this.byuId}`)
    this.favoriteColor = response.favoriteColor
    console.log(response.favoriteColor)
  }
}
</script>
