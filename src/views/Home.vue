<template>
  <div class="home">
    <h1>All Contacts</h1>
    <p>First Name: <input type="text" v-model="newContactFirstName"></p>
    <p>Last Name: <input type="text" v-model="newContactLastName"></p>
    <p>Email: <input type="text" v-model="newContactEmail"></p>
    <p>Phone Number: <input type="text" v-model="newContactPhoneNumber"></p>
    <button v-on:click="createContact()">Create Contact</button>
    <hr>
    <div v-for="contact in contacts">
      <p>{{ contact.first_name }}</p>
      <p>{{ contact.last_name }}</p>
  
      <button v-on:click="showContact(contact)">Show more</button>
      <div v-if="currentContact === contact">
        <p>{{ contact.email }}</p>
        <p>{{ contact.phone_number }}</p>

        <p>First Name: <input type="text" v-model="contact.first_name"></p>
        <p>Last Name: <input type="text" v-model="contact.last_name"></p>
        <p>Email: <input type="text" v-model="contact.email"></p>
        <p>Phone Number: <input type="text" v-model="contact.phone_number"></p>
        <button v-on:click="updateContact(contact)">Update Contact</button>
        <button v-on:click="destroyContact(contact)">Delete Contact</button>
      </div>
      <hr>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from "axios";

export default {
  data: function() {
    return {
      contacts: [],
      currentContact: {},
      newContactFirstName: "",
      newContactLastName: "",
      newContactEmail: "",
      newContactPhoneNumber: ""
    };
  },
  created: function() {
    axios.get('/api/contacts').then(response => {
      this.contacts = response.data;
    });
  },
  methods: {
    createContact: function() {
      var params = {
        first_name: this.newContactFirstName,
        last_name: this.newContactLastName,
        email: this.newContactEmail,
        phone_number: this.newContactPhoneNumber
      };
      axios.post('/api/contacts', params).then(response => {
        this.contacts.push(response.data);
        this.newContactFirstName = "";
        this.newContactLastName = "";
        this.newContactEmail = "";
        this.newContactPhoneNumber = "";
      });
    },
    showContact: function(contact) {
      if (this.currentContact === contact) {
        this.currentContact = {};
      } else {
        this.currentContact = contact;
      }
    },
    updateContact: function(contact) {
      var params = {
        first_name: contact.first_name,
        last_name: contact.last_name,
        email: contact.email,
        phone_number: contact.phone_number
      };
      console.log('in the update contact');
      axios.patch('/api/contacts/' + contact.id, params).then(response => {
        this.currentContact = {};
      });
    },
    destroyContact: function(contact) {
      console.log('deleting the contact');
      axios.delete('/api/contacts/' + contact.id + ".json").then(response => {
        console.log(response.data);
        var index = this.contacts.indexOf(contact);
        this.contacts.splice(index, 1);
      });
    }
  }
};
</script>
