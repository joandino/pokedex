<template>
  <v-item-group>
    <v-container>
      <v-row>
        <v-col
          v-for="pokemon in pokemons"
          :key="pokemon.name"
          cols="12"
          md="3"
        >
          <v-item>
            <v-card
              class="mx-auto"
              max-width="344"
            >
              <div
                :style="{
                  'background-image': `url(${pokemon.sprites.other['official-artwork'].front_default})`,
                  height: '200px',
                  'background-size': 'contain',
                  'background-position': 'center',
                }"
              ></div>

              <v-card-title>
                <h3>{{pokemon.name}}</h3>
              </v-card-title>

              <v-card-subtitle>
                <v-chip
                  class="ma-2"
                  :color="getTypeColor(type.type)"
                  v-for="type in pokemon.types"
                  v-bind:key="type.type.name"
                >
                  {{type.type.name}}
                </v-chip>
              </v-card-subtitle>
    
              <v-card-actions>
                <v-btn
                  color="orange lighten-2"
                  text
                  @click="openDialog(pokemon)"
                >
                  View
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-item>
        </v-col>
      </v-row>

      <div style="text-align: center; margin-top: 20px;">
        <v-btn
          class="ma-2"
          color="secondary"
          :loading="loadingMorePokemons"
          @click="loadMorePokemons()"
        >
          +10 Pokemons More
        </v-btn>
      </div>

      <PokeCard 
        :pokeinfo="pokemonSelected"
        :showDialog="showDialog"
        @closeDialog="closeDialog"
      />

      <v-overlay :value="overlay">
        <v-progress-circular
          indeterminate
          size="64"
        ></v-progress-circular>
      </v-overlay>
    </v-container>
  </v-item-group>
</template>

<script lang="ts">
import Vue from "vue";
import axios from "axios";
import pokeTypes from "../../pokeTypes.json";
import PokeCard from "@/components/PokeCard.vue";

export default Vue.extend({
  name: 'Home',
    components: { PokeCard },
    data() {
        return {
            pokeApiUrl: process.env.VUE_APP_API_URL,
            pokemons: [] as any,
            overlay: false,
            loadingMorePokemons: false,
            pokemonTypes: pokeTypes,
            pokemonQuantity: sessionStorage.getItem('pokemonQuantity') ? JSON.parse(sessionStorage.getItem('pokemonQuantity') || '{}') : 151,
            pokemonSelected: {},
            showDialog: false,
        }
    },
    methods: {
      async getPokemons() {
        let res = await axios.get(`${this.pokeApiUrl}/pokemon?limit=${this.pokemonQuantity}`);

        if(res.data){
          let dataArray = [];

          this.overlay = true;
          
          for(const pokemon of res.data.results){
            let resData = await axios.get(`${pokemon.url}`);

            dataArray.push(resData.data);
          }

          this.pokemons = dataArray;

          this.overlay = false;
        }
      },
      getTypeColor(_type:any) {
        let color = "";

        for(let type of this.pokemonTypes) {
          if(_type.name === type.name) {
            color = type.color;
            break;
          }
        }

        return color;
      },
      async loadMorePokemons() {
        let tempArray = [];
        this.loadingMorePokemons = true;

        for(let i=this.pokemonQuantity+1; i<=this.pokemonQuantity+10; i++){
          let res = await axios.get(`${this.pokeApiUrl}/pokemon/${i}`);

          tempArray.push(res.data);
        }

        this.pokemonQuantity = this.pokemonQuantity + 10;
        sessionStorage.setItem('pokemonQuantity', this.pokemonQuantity);
        this.pokemons.push(...tempArray);
        this.loadingMorePokemons = false;
      },

      openDialog(_pokemon: any) {
        this.pokemonSelected = _pokemon;
        this.showDialog = true;
      },

      closeDialog(close:boolean){
        if(close){
          this.showDialog = false;
        } else {
          this.showDialog = true;
        }
      }
    },
    created() {
        this.getPokemons();
    }
})
</script>

<style>
body {
  background-image: url('~@/assets/pokeball.jpg');
  background-repeat: repeat;
  background-attachment: fixed;
  height: 100vh;
}
h3::first-letter {
  text-transform: capitalize;
}
</style>