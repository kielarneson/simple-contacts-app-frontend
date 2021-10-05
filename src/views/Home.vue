<template>
  <div class="home">
    <h1>Contacts</h1>
    <div v-for="contact in contacts" v-bind:key="contact.id">
      <h2>{{ contact.first_name }} {{ contact.last_name }}</h2>
      <img :src="contact.image" alt="" />
      <p>{{ contact.email }}</p>
      <p>{{ contact.phone_number }}</p>
    </div>
  </div>
</template>

<style>
img {
  width: 200px;
}
</style>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      contacts: [],
    };
  },
  created: function () {
    this.indexContacts();
  },
  methods: {
    indexContacts: function () {
      axios.get("/contacts").then((response) => {
        console.log("Photos index", response.data);
        this.contacts = response.data;
      });
    },
  },
};
</script>
