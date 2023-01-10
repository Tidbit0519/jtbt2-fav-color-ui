<template>
  <v-container class="grey lighten-5">
    <v-dialog width="600px">
      <template #activator="{ on, attrs }">
        <div style="padding: 1rem">
          <v-btn
            v-bind="attrs"
            v-on="on">
            Add
          </v-btn>
        </div>
      </template>
      <v-card>
        <v-card-title>
          Add a new person!
        </v-card-title>
        <div style="padding: 1rem">
          <v-text-field
            label="Name"
            v-model="newStudentName"
          />
          <v-text-field
            label="BYU ID"
            v-model="newByuId"
          />
          <v-select
            v-model="newFavColor"
            :items="options"
            label="Colors"
          />
          <v-card-actions>
            <v-btn
              @click="addFavoriteColor"
            >
              Save
            </v-btn>
          </v-card-actions>
        </div>
      </v-card>
    </v-dialog>
    <v-row
      align-content="align"
      no-gutters>
        <v-col
          :key="item.byuId"
          v-for="item in students"
          cols="12"
          sm="4"
        >
          <FavoriteColorCard
            :name="item.name"
            :byu-id="item.byuId"
            :fav-color="item.favoriteColor"
            style="border-radius: 1rem; margin: 1rem"
          >
          </FavoriteColorCard>
        </v-col>
    </v-row>
  </v-container>
</template>

<script lang="ts">
import { Component, Vue } from 'nuxt-property-decorator'
import FavoriteColorCard from '~/components/FavoriteColorCard.vue'

@Component({
  components: {
    FavoriteColorCard
  }
})

export default class IndexPage extends Vue {
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

  newStudentName: string = ''
  newByuId: string = ''
  newFavColor: string = ''
  students = []

  async mounted () {
    const response = await this.$axios.$get('https://jtbt2-fav-color-dev.byu-oit-fullstack-trn.amazon.byu.edu/')
    this.students.push.apply(this.students, response)
  }

  async addFavoriteColor () {
    await this.$axios.$post('https://jtbt2-fav-color-dev.byu-oit-fullstack-trn.amazon.byu.edu/', {
      byuId: this.newByuId,
      name: this.newStudentName,
      favoriteColor: this.newFavColor
    })
    this.$router.go(0)
  }
}
</script>
