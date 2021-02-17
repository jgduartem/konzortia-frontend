<template>
  <div class="row col-12 bg-grey-2" style="height: 100vh">
    <div class="col-xs-12 col-sm-12 col-md-4 col-lg-4 q-pa-md">
      <q-card class="my-card">
        <q-card-section>
          <div class="text-h6 text-center">Add Movie</div>
        </q-card-section>
        <q-card-section>
          <q-input class="q-ma-md" v-model="name" type="text" label="Name" />
          <q-input
            class="q-ma-md"
            v-model="director"
            type="text"
            label="Director"
          />
          <q-input
            class="q-ma-md"
            v-model="clasification"
            type="text"
            label="Clasification"
          />
        </q-card-section>
        <q-card-actions class="row col-12 justify-end" horizontal>
          <q-btn class="q-ma-md" flat label="Save" @click="addMovie()" :disable='name == ""'/>
        </q-card-actions>
      </q-card>
    </div>
    <div class="col-xs-12 col-sm-12 col-md-8 col-lg-8 q-pa-md">
      <q-table
        title="Movies"
        :data="data"
        :columns="columns"
        row-key="name"
        selection="single"
        :selected.sync="selected"
      >
        <template v-slot:top>
          <div class="row col-12 justify-end">
            <q-btn
            class="q-ma-sm"
              color="primary"
              label="Edit Movie"
              @click="openModal()"
              :disable="selected.length <= 0"
            />
            <q-btn
              class="q-ma-sm"
              color="primary"
              label="Delete Movie"
              @click="deleteMovie()"
              :disable="selected.length <= 0"
            />
          </div>
        </template>
      </q-table>
    </div>
    <q-dialog v-model="modal" persistent>
      <q-card class="my-card">
        <q-card-section>
          <div class="text-h6 text-center">Edit Movie</div>
        </q-card-section>
        <q-card-section>
          <q-input
            class="q-ma-md"
            v-model="editName"
            type="text"
            label="Name"
          />
          <q-input
            class="q-ma-md"
            v-model="editDirector"
            type="text"
            label="Director"
          />
          <q-input
            class="q-ma-md"
            v-model="editClasi"
            type="text"
            label="Clasification"
          />
        </q-card-section>
        <q-card-actions class="row col-12 justify-end" horizontal>
          <q-btn class="q-ma-md" flat label="Cancel" v-close-popup @click="cleanInputs()" />
          <q-btn class="q-ma-md" flat label="Save" @click="editMovie()" />
        </q-card-actions>
      </q-card>
    </q-dialog>
  </div>
</template>

<script>
import Axios from "axios";
export default {
  name: "Movies",
  props: {},
  data() {
    return {
      data: [],
      selected: [],
      name: "",
      director: "",
      clasification: "",
      editName: "",
      editClasi: "",
      editDirector: "",
      modal: false,
      columns: [
        {
          name: "_id",
          label: "ID",
          align: "left",
          field: "_id",
        },
        {
          name: "name",
          label: "Name",
          align: "left",
          field: "name",
        },
        {
          name: "director",
          label: "Director",
          align: "left",
          field: "director",
        },
        {
          name: "clasification",
          label: "Clasification",
          align: "left",
          field: "clasification",
        },
      ],
    };
  },
  methods: {
    async getMovies() {
      const moviesUrl = process.env.MOVIES_URL
        ? process.env.MOVIES_URL
        : "http://localhost:4000/movies/";
      try {
        const moviesData = await Axios({
          method: "get",
          url: moviesUrl,
          headers: {
            "Content-Type": "application/json",
          },
        });
        return moviesData.data;
      } catch (err) {
        console.log(err);
      }
    },
    async addMovie() {
      const moviesUrl = process.env.MOVIES_URL
        ? process.env.MOVIES_URL
        : "http://localhost:4000/movies/";
      try {
        await Axios({
          method: "post",
          url: moviesUrl,
          data: {
            name: this.name,
            director: this.director,
            clasification: this.clasification,
          },
          headers: {
            "Content-Type": "application/json",
          },
        });
        this.data = await this.getMovies();
        this.cleanInputs();
      } catch (err) {
        console.log(err);
      }
    },
    openModal() {
      this.modal = true;
      this.editName = this.selected[0].name;
      this.editDirector = this.selected[0].director;
      this.editClasi = this.selected[0].clasification;
    },
    async editMovie() {
      const moviesUrl = process.env.MOVIES_URL
        ? process.env.MOVIES_URL + this.selected[0]._id
        : "http://localhost:4000/movies/" + this.selected[0]._id;
      try {
        await Axios({
          method: "put",
          url: moviesUrl,
          data: {
            name: this.editName,
            director: this.editDirector,
            clasification: this.editClasi,
          },
          headers: {
            "Content-Type": "application/json",
          },
        });
        this.data = await this.getMovies();
        this.cleanInputs();
        this.modal = false;
      } catch (err) {
        console.log(err);
      }
    },
    async deleteMovie() {
      const moviesUrl = process.env.MOVIES_URL
        ? process.env.MOVIES_URL + this.selected[0]._id
        : "http://localhost:4000/movies/" + this.selected[0]._id;
      try {
        await Axios({
          method: "delete",
          url: moviesUrl,
        });
        this.data = await this.getMovies();
      } catch (err) {
        console.log(err);
      }
    },
    cleanInputs() {
      this.name = "";
      this.director = "";
      this.clasification = "";
      this.editClasi = "";
      this.editName = "";
      this.editDirector = "";
      this.selected = [];
    },
  },
  async created() {
    this.data = await this.getMovies();
    console.log(this.data);
  },
};
</script>

<style>
</style>