<template>
  <div class="home">
    <div>
      <h1>Create Contact</h1>
      First Name:
      <input type="text" v-model="createContactParams.first_name" />
      Last Name:
      <input type="text" v-model="createContactParams.last_name" />
      Email:
      <input type="text" v-model="createContactParams.email" />
      Phone Number:
      <input type="text" v-model="createContactParams.phone_number" />
      <button @click="createContacts()">Create</button>
    </div>

    <h1>Contacts</h1>
    <div v-for="contact in contacts" v-bind:key="contact.id">
      <h2>{{ contact.first_name }} {{ contact.last_name }}</h2>
      <img v-bind:src="contact.image" alt="" />
      <p>Email: {{ contact.email }}</p>
      <p>Phone Number: {{ contact.phone_number }}</p>
      <div>
        <button @click="showContact(contact)">More Info</button>
      </div>
    </div>

    <dialog id="contact-details">
      <form method="dialog">
        <h1>Contact Info</h1>
        First Name:
        <input type="text" v-model="currentContact.first_name" />
        Last Name:
        <input type="text" v-model="currentContact.last_name" />
        Email:
        <input type="text" v-model="currentContact.email" />
        Phone Number:
        <input type="text" v-model="currentContact.phone_number" />
        <button @click="updateContact(currentContact)">Update</button>
        <button @click="deleteContact(currentContact)">Delete</button>
        <button>Close</button>
      </form>
    </dialog>
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
      createContactParams: {},
      currentContact: {},
    };
  },
  created: function () {
    this.indexContacts();
  },
  methods: {
    indexContacts: function () {
      axios.get("/contacts").then((response) => {
        console.log("Index Contacts", response.data);
        this.contacts = response.data;
      });
    },
    createContacts: function () {
      axios.post("/contacts.json", this.createContactParams).then((response) => {
        console.log("Create Contacts", response);
        this.contacts.push(response.data);
        this.createContactParams = {};
      });
    },
    showContact: function (contact) {
      this.currentContact = contact;
      document.querySelector("#contact-details").showModal();
    },
    updateContact: function (contact) {
      var updateContactParams = contact;
      axios.patch(`/contacts/${contact.id}.json`, updateContactParams).then((response) => {
        console.log("Successful Update", response.data);
      });
    },
    deleteContact: function (contact) {
      axios
        .delete(`/contacts/${contact.id}.json`)
        .then((response) => {
          console.log("Contact successfully deleted", response);
          var index = this.contacts.indexOf(contact);
          this.contacts.splice(index, 1);
        })
        .catch((error) => {
          console.log("photos create error", error.response);
        });
    },
  },
};
</script>
