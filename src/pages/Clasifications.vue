<template>
  <div class="row col-12 bg-grey-2" style="height: 100vh">
    <div class="col-4 q-pa-md">
      <q-card class="my-card">
        <q-card-section>
          <div class="text-h6 text-center">Add Clasification</div>
        </q-card-section>
        <q-card-section>
          <q-input class="q-ma-md" v-model="name" type="text" label="Name" />
        </q-card-section>
        <q-card-actions class="row col-12 justify-end" horizontal>
          <q-btn
            class="q-ma-md"
            flat
            label="Save"
            @click="addClasification()"
            :disable="name == ''"
          />
        </q-card-actions>
      </q-card>
    </div>
    <div class="col-8 q-pa-md">
      <q-table
        title="Clasifications"
        :data="data"
        :columns="columns"
        row-key="name"
        selection="single"
        :selected.sync="selected"
      >
        <template v-slot:top>
          <div class="row col-12 justify-end">
            <q-btn
              color="primary"
              label="Edit Clasification"
              @click="openModal()"
              :disable="selected.length <= 0"
            />
            <q-btn
              class="q-ml-sm"
              color="primary"
              label="Delete Clasification"
              @click="deleteClasification()"
              :disable="selected.length <= 0"
            />
          </div>
        </template>
      </q-table>
    </div>
    <q-dialog v-model="modal" persistent>
      <q-card class="my-card">
        <q-card-section>
          <div class="text-h6 text-center">Edit Clasification</div>
        </q-card-section>
        <q-card-section>
          <q-input
            class="q-ma-md"
            v-model="editName"
            type="text"
            label="Name"
          />
        </q-card-section>
        <q-card-actions class="row col-12 justify-end" horizontal>
          <q-btn
            class="q-ma-md"
            flat
            label="Cancel"
            v-close-popup
            @click="cleanInputs()"
          />
          <q-btn
            class="q-ma-md"
            flat
            label="Save"
            @click="editClasification()"
          />
        </q-card-actions>
      </q-card>
    </q-dialog>
  </div>
</template>

<script>
import Axios from "axios";
export default {
  name: "Clasifications",
  props: {},
  data() {
    return {
      name: "",
      selected: [],
      data: [],
      editName: "",
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
          label: "Clasification",
          align: "left",
          field: "name",
        },
      ],
    };
  },
  methods: {
    async getClasifications() {
      const clasiURL = process.env.CLASIFICATIONS_URL
        ? process.env.CLASIFICATIONS_URL
        : "http://localhost:4000/clasifications/";
      try {
        const clasiData = await Axios({
          method: "get",
          url: clasiURL,
          headers: {
            "Content-Type": "application/json",
          },
        });
        return clasiData.data;
      } catch (err) {
        console.log(err);
      }
    },
    async addClasification() {
      const clasiURL = process.env.CLASIFICATIONS_URL
        ? process.env.CLASIFICATIONS_URL
        : "http://localhost:4000/clasifications/";
      try {
        await Axios({
          method: "post",
          url: clasiURL,
          data: {
            name: this.name,
          },
          headers: {
            "Content-Type": "application/json",
          },
        });
        this.data = await this.getClasifications();
        this.name = "";
      } catch (err) {
        console.log(err);
      }
    },
    async editClasification() {
      const clasiURL = process.env.CLASIFICATIONS_URL
        ? process.env.CLASIFICATIONS_URL + this.selected[0]._id
        : "http://localhost:4000/clasifications/" + this.selected[0]._id;
      try {
        await Axios({
          method: "put",
          url: clasiURL,
          data: {
            name: this.editName,
          },
          headers: {
            "Content-Type": "application/json",
          },
        });
        this.data = await this.getClasifications();
        this.cleanInputs();
        this.modal = false;
      } catch (err) {
        console.log(err);
      }
    },
    async deleteClasification() {
      const clasiURL = process.env.CLASIFICATIONS_URL
        ? process.env.CLASIFICATIONS_URL + this.selected[0]._id
        : "http://localhost:4000/clasifications/" + this.selected[0]._id;
      try {
        await Axios({
          method: "delete",
          url: clasiURL,
        });
        this.data = await this.getClasifications();
      } catch (err) {
        console.log(err);
      }
    },
    openModal() {
      this.modal = true;
      this.editName = this.selected[0].name;
    },
    cleanInputs() {
      this.name = "";
      this.editName = "";
    },
  },
  async created() {
    this.data = await this.getClasifications();
    console.log(this.data);
  },
};
</script>

<style>
</style>