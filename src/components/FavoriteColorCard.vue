<template>
  <v-card
    max-width="700"
    outlined
  >
    <v-list-item>
      <v-list-item-content>
        <v-list-item-title class="text-h4">{{ name }}</v-list-item-title>
        <br>
        <v-list-item-subtitle>BYU ID: {{ byuId }}</v-list-item-subtitle>
        <br>
        <v-list-item-subtitle>Favorite Color: {{ color }}</v-list-item-subtitle>
      </v-list-item-content>

      <v-list-item-avatar
        tile
        size="100"
        :color="color"
      />
    </v-list-item>

    <v-row no-gutters>
      <v-dialog width="600px">
        <template #activator="{ on, attrs }">
          <div style="padding: 1rem">
            <v-btn
              color="primary"
              dark
              v-bind="attrs"
              v-on="on">
              Edit</v-btn>
          </div>
        </template>
        <v-card>
          <v-card-title>
            Pick a color!
          </v-card-title>
          <div style="padding: 1rem">
            <v-select
              v-model=color
              :items="options"
              label="Colors"
              @change = changeColor()
            />
          </div>
        </v-card>
      </v-dialog>

      <v-dialog max-width="600px">
        <template #activator="{ on, attrs }">
          <div style="padding: 1rem">
            <v-btn
              color="error"
              dark
              v-bind="attrs"
              v-on="on"
              @click=deleteFavoriteColor>
              Delete</v-btn>
          </div>
        </template>
        <v-card>
          <v-card-title>
            Deleting color...
<!--            Are you sure you want to delete this color?-->
          </v-card-title>
<!--              <div style="padding: 1rem">-->
<!--                <v-btn-->
<!--                  @click="deleteFavoriteColor">-->
<!--                  Yes-->
<!--                </v-btn>-->
<!--                <v-btn-->
<!--                  @click = closeDialog>-->
<!--                  No-->
<!--                </v-btn>-->
<!--              </div>-->
        </v-card>
      </v-dialog>
    </v-row>
  </v-card>

</template>

<script lang="ts">
import { Component, Vue } from 'nuxt-property-decorator'
import { Prop } from 'vue-property-decorator'

@Component
export default class FavoriteColorCard extends Vue {
  @Prop({ required: true })
    name?: string

  @Prop({ required: true })
    byuId?: string

  @Prop({ required: true })
    favColor?: string

  color: string = ''

  mounted () {
    this.color = this.favColor ?? ''
  }

  async changeColor () {
    console.log(this.color)
    await this.$axios.put(`https://jtbt2-fav-color-dev.byu-oit-fullstack-trn.amazon.byu.edu/${this.byuId}`, {
      favoriteColor: this.color
    })
    this.$router.go(0)
  }

  deleteFavoriteColor = async () => {
    await this.$axios.delete(`https://jtbt2-fav-color-dev.byu-oit-fullstack-trn.amazon.byu.edu/${this.byuId}`)
    this.$router.go(0)
  }

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
}
</script>
