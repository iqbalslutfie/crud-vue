<template>
  <div class="container">
    <h1 class="mt-3">
      Demo Tes VUE
      <small>Iqbal Syarifudin Lutfie</small>
    </h1>
    <form @submit.prevent="add" class="mt-5">
      <div class="form-group col-md-4">
        <input type="hidden" v-model="form.id" class="form-control" />
        <label for>Insert Name</label>
        <input type="text" v-model="form.name" class="form-control" />
      </div>
      <div class="form-group col-md-4">
        <button type="submit" v-show="!updateSubmit" class="btn btn-md btn-primary">Add</button>
        <!-- jika tidak update maka tombol update tidak muncul -->
        <button
          type="button"
          v-show="updateSubmit"
          @click="update(form)"
          class="btn btn-md btn-primary"
        >Update</button>
        <!-- jika tombol edit di klik maka tombol add akan berubah menjadi update -->
      </div>
    </form>

    <table class="table table-bordered mt-4">
      <thead class="thead-dark text-center">
        <th width="7%">No</th>
        <th>Name</th>
        <th width="20%">Action</th>
      </thead>
      <tbody>
        <tr v-for="user in users" :key="user.id">
          <td class="text-center">{{user.id}}</td>
          <td>
            <span>{{user.name}}</span> &#160;
          </td>
          <td class="text-center">
            <button @click="edit(user)" class="btn btn-sm btn-info">Edit</button> ||
            <button @click="del(user)" class="btn btn-sm btn-danger">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
/* eslint-disable */

import axios from "axios";

export default {
  data() {
    return {
      form: {
        id: "",
        name: ""
      },
      users: "",
      updateSubmit: false
    };
  },
  mounted() {
    this.load();
  },
  methods: {
    add() {
      axios.post("http://localhost:3000/users/", this.form).then(res => {
        this.load();
        this.form.name = "";
      });
    },
    edit(user) {
      this.updateSubmit = true;
      this.form.id = user.id;
      this.form.name = user.name;
    },
    update(form) {
      return axios
        .put("http://localhost:3000/users/" + form.id, { name: this.form.name })
        .then(res => {
          this.load();
          this.form.id = "";
          this.form.name = "";
          this.updateSubmit = false;
        })
        .catch(err => {
          console.log(err);
        });
    },
    del(user) {
      axios.delete("http://localhost:3000/users/" + user.id).then(res => {
        this.load();
        let index = this.users.indexOf(form.name);
        this.users.splice(index, 1);
      });
    },
    load() {
      axios
        .get("http://localhost:3000/users")
        .then(res => {
          this.users = res.data; //respon dari rest api dimasukan ke users
        })
        .catch(err => {
          console.log(err);
        });
    }
  }
};
</script>