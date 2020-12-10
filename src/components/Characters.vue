<template >
  <div>
    <div class="container">
      <div class="mt-5">
        <b-pagination
          v-model="currentPage"
          :total-rows="infocharacters.count"
          :per-page="20"
          @change="onPageChanged"
          align="right"
          size="lg"
          first-number
          last-number
        ></b-pagination>
      </div>
      <b-row>
        <b-col md="4" v-for="(character, index) in characters" :key="index">
          <b-card
            :title="character.name"
            :img-src="character.image"
            img-alt="Image"
            img-top
            tag="article"
            style="max-width: 20rem"
            class="mt-3"
          >
            <b-card-text>
              Especie: {{ character.species }} <br />
              Genero: {{ character.gender }} <br />
              Estatus: {{ character.status }} <br />
              Origen: {{ character.origin.name }} <br />
            </b-card-text>

            <b-button href="#" variant="primary">Go somewhere</b-button>
          </b-card>
        </b-col>
      </b-row>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Characters",
  data() {
    return {
      characters: [],
      infocharacters: [],
      currentPage: 1,
    };
  },
  created: function () {
    this.getCharacters(1);
  },
  computed: {},
  methods: {
    getCharacters: async function (currentPage) {
      try {
        this.characters = [];
        let res = await axios.get(
          `https://rickandmortyapi.com/api/character?page=${currentPage}`
        );
        this.characters = res.data.results;
        this.infocharacters = res.data.info;
        //console.log(this.infocharacters);
      } catch (error) {
        console.log(error);
      }
    },
    onPageChanged: function (page) {
      console.log(page);
      this.getCharacters(page);
    },
  },
};
</script>