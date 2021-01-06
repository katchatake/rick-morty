<template>
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

            <b-button
              href="#"
              block
              variant="outline-success"
              size="lg"
              @click="see(character)"
              >Ver más</b-button
            >
          </b-card>
        </b-col>
      </b-row>
    </div>
    <b-modal id="modal-character" size="lg" hide-footer>
      <div align="center">
        <b-card no-body class="overflow-hidden" style="max-width: 750px">
          <b-row no-gutters>
            <b-col md="5">
              <b-card-img
                :src="character.image"
                :alt="character.name"
                class="rounded-0"
              ></b-card-img>
            </b-col>
            <b-col md="7">
              <b-card-body :title="character.name">
                <b-card-text>
                  <b-form-group
                    label-cols-sm="2"
                    label="Estatus"
                    label-align-sm="center"
                    label-for="nested-street"
                  >
                    <b-form-input
                      disabled
                      v-model="character.status"
                    ></b-form-input>
                  </b-form-group>
                  <b-form-group
                    label-cols-sm="2"
                    label="Especie"
                    label-align-sm="center"
                    label-for="nested-street"
                  >
                    <b-form-input
                      disabled
                      v-model="character.species"
                    ></b-form-input>
                  </b-form-group>
                  <b-form-group
                    label-cols-sm="2"
                    label="Tipo"
                    label-align-sm="center"
                    label-for="nested-street"
                  >
                    <b-form-input
                      disabled
                      v-model="character.type"
                    ></b-form-input>
                  </b-form-group>
                  <b-form-group
                    label-cols-sm="2"
                    label="Genero"
                    label-align-sm="center"
                    label-for="nested-street"
                  >
                    <b-form-input
                      disabled
                      v-model="character.gender"
                    ></b-form-input>
                  </b-form-group>
                  <router-link :to="'/character/'+character.id">
                  <b-button
                    block variant="outline-primary"
                    >Ver a detalle</b-button
                  >
                  </router-link>
                </b-card-text>
              </b-card-body>
            </b-col>
          </b-row>
        </b-card>
      </div>
      <b-button
        class="mt-3"
        variant="outline-danger"
        block
        @click="$bvModal.hide('modal-character')"
        >Cerrar</b-button
      >
    </b-modal>
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
      character: {
        id: 0,
        name: "",
        status: "",
        species: "",
        type: "",
        gender: "",
        origin: {},
        location: {},
        image: "",
        episode: {},
        url: "",
        created: "",
      },
    };
  },
  created: function () {
    this.getCharacters(1);
  },
  computed: {},
  methods: {
    see: function (character) {
      this.character.id = character.id;
      this.character.name = character.name;
      this.character.status = character.status;
      this.character.species = character.species;
      this.character.type = character.type;
      this.character.gender = character.gender;
      this.character.origin = character.origin;
      this.character.location = character.location;
      this.character.image = character.image;
      this.character.episode = character.episode;
      this.character.url = character.url;
      this.character.created = character.created;
      this.$bvModal.show("modal-character");
    },
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
