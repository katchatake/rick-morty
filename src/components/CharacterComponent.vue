<template>
  <div>
    <b-jumbotron
      fluid
      :header="character.name"
      :lead="`${character.gender} | ${character.species} | ${character.status}`"
    >
      <b-row>
        <b-col align="left">
          <b-img rounded :src="character.image"></b-img>
        </b-col>
        <b-col aling="left">
          <b-table stacked :items="characterDetail"></b-table>
          <b-button
            size="lg"
            block
            :variant="outlinebutton"
            @click="episodesCharacter()"
            >{{ buttonEpisode }}</b-button
          >
        </b-col>
      </b-row>
    </b-jumbotron>
    <b-modal id="modal-episodes" size="lg" hide-footer>
      <div align="center">
        <div id="spinner-episodes" style="display: block;">
          <b-spinner class="m-5" label="Busy"></b-spinner>
        </div>
        <template>
          <div
            class="accordion"
            id="accordion-episodes"
            style="display: none;"
            role="tablist"
          >
            <b-card
              v-for="(episode, index) in episodes"
              :key="index"
              no-body
              class="mb-1"
            >
              <b-card-header header-tag="header" class="p-1" role="tab">
                <b-button block v-b-toggle="'accordion' + index" variant="info"
                  >Episode -> | {{ episode.episode }} |
                </b-button>
              </b-card-header>
              <b-collapse
                :id="'accordion' + index"
                accordion="my-accordion"
                role="tabpanel"
              >
                <b-card-body>
                  <b-card-text> Name : {{ episode.name }}</b-card-text>
                  <b-card-text> Air Date : {{ episode.air_date }} </b-card-text>
                </b-card-body>
              </b-collapse>
            </b-card>
          </div>
        </template>
      </div>
      <b-button
        class="mt-3"
        variant="outline-danger"
        block
        @click="$bvModal.hide('modal-episodes')"
        >Cerrar</b-button
      >
    </b-modal>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "CharacterComponent",
  data() {
    return {
      character: [],
      episodes: [],
      buttonEpisode: "Episodes",
      outlinebutton: "outline-success",
      characterDetail: [
        {
          origin: "",
          location: "",
          created: "",
        },
      ],
    };
  },
  created: function() {
    this.getCharacter();
  },
  methods: {
    getDetail: function(character) {
      this.characterDetail[0].origin = character.origin.name;
      this.characterDetail[0].location = character.location.name;
      this.characterDetail[0].created = character.created;
    },
    episodesCharacter: async function() {
      this.buttonEpisode = "Cargando...";
      this.outlinebutton = "outline-warning";
      this.$bvModal.show("modal-episodes");
      for (let index = 0; index < this.character.episode.length; index++) {
        try {
          let res = await axios.get(this.character.episode[index]);
          //console.log(res.data);
          this.episodes.push(res.data);
        } catch (error) {
          console.log(error);
        }
      }
      document.getElementById("accordion-episodes").style.display = "block";
      document.getElementById("spinner-episodes").style.display = "none";
      this.outlinebutton = "outline-success";
    },
    getCharacter: async function() {
      try {
        let res = await axios.get(
          `https://rickandmortyapi.com/api/character/${this.$route.params.id}`
        );
        console.log(res.data);
        this.character = res.data;
        this.getDetail(res.data);
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>
