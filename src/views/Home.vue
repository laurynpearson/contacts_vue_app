<template>
  <div class="home">
    <h1>All Contacts</h1>
    <div v-for="contact in contacts">
      <p>{{ contact.first_name }}</p>
      <p>{{ contact.last_name }}</p>
      <p>{{ contact.email }}</p>
      <p>{{ contact.phone_number }}</p>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from "axios";

export default {
  data: function() {
    return {
      contacts: []
    };
  },
  created: function() {
    axios.get('/api/contacts').then(response => {
      this.contacts = response.data;
    });
  },
  methods: {}
};
</script>
