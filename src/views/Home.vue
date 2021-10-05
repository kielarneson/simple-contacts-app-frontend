<template>
  <div class="home">
    <h1>Create Contact</h1>
    <div>
      <div>
        First Name:
        <input type="text" v-model="newContactParams.first_name" />
      </div>
      <div>
        Last Name:
        <input type="text" v-model="newContactParams.last_name" />
      </div>
      <div>
        Email:
        <input type="text" v-model="newContactParams.email" />
      </div>
      <div>
        Phone Number:
        <input type="text" v-model="newContactParams.phone_number" />
      </div>
      <button v-on:click="createContacts()">Create</button>
    </div>
    <h1>Contacts</h1>
    <div v-for="contact in contacts" v-bind:key="contact.id">
      <h2>{{ contact.first_name }} {{ contact.last_name }}</h2>
      <img :src="contact.image" alt="" />
      <p>{{ contact.email }}</p>
      <p>{{ contact.phone_number }}</p>
      <button v-on:click="showContact(contact)">More Info</button>
    </div>

    <dialog id="contact-details">
      <form method="dialog">
        <h1>Contact Info</h1>
        <div>
          First Name:
          <input type="text" v-model="currentContact.first_name" />
        </div>
        <div>
          Last Name:
          <input type="text" v-model="currentContact.last_name" />
        </div>
        <div>
          Email:
          <input type="text" v-model="currentContact.email" />
        </div>
        <div>
          Phone Number:
          <input type="text" v-model="currentContact.phone_number" />
        </div>
        <button v-on:click="updateContact(currentContact)">Update</button>
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
      newContactParams: {},
      currentContact: {},
    };
  },
  created: function () {
    this.indexContacts();
  },
  methods: {
    indexContacts: function () {
      axios.get("/contacts").then((response) => {
        console.log("Contacts index", response.data);
        this.contacts = response.data;
      });
    },
    createContacts: function () {
      axios.post("/contacts.json", this.newContactParams).then((response) => {
        console.log("Contacts create", response);
        this.contacts.push(response.data);
        this.newContactParams = {};
      });
    },
    showContact: function (contact) {
      this.currentContact = contact;
      document.querySelector("#contact-details").showModal();
    },
    updateContact: function (contact) {
      var updateContactParams = contact;
      axios.patch(`/contacts/${contact.id}.json`, updateContactParams).then((response) => {
        console.log("Update contact", response);
        this.currentContact = {};
      });
    },
  },
};
</script>
