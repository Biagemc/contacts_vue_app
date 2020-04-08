<template>
  <div class="home">
    <h1>{{ message }}</h1>

    <div>
      <p>
        First Name:
        <input type="text" v-model="newContactFirstName" />
      </p>
      <p>
        Middle Name:
        <input type="text" v-model="newContactMiddleName" />
      </p>
      <p>
        Last Name:
        <input type="text" v-model="newContactLastName" />
      </p>
      <p>
        Email:
        <input type="text" v-model="newContactEmail" />
      </p>
      <p>
        Phone Number:
        <input type="text" v-model="newContactPhoneNumber" />
      </p>
      <p>
        Bio:
        <input type="text" v-model="newContactBio" />
      </p>
      <p>
        Address:
        <input type="text" v-model="newContactAddress" />
      </p>

      <button v-on:click="addContact()">Create a contact</button>
      <hr />
    </div>
    <div v-bind:key="contact.id" v-for="contact in contacts">
      <li>{{contact.id}} {{ contact.name}}</li>
      <p>Email: {{ contact.email}}</p>
      <p>Phone Number: {{ contact.phone_number}}</p>
      <button v-on:click="showContact(contact)">Show info</button>
      <button v-on:click="deleteContact(contact)">Delete Contact</button>
      <div v-if="currentContact === contact">
        <p>First Name: {{contact.first_name}}</p>
        <p>Middle Name: {{contact.middle_name}}</p>
        <p>Last Name: {{contact.last_name}}</p>
        <p>Bio: {{contact.bio}}</p>
        <p>Address: {{contact.address}}</p>
        <div>
          <p>
            First Name:
            <input type="text" v-model="contact.first_name" />
          </p>
          <p>
            Middle Name:
            <input type="text" v-model="contact.middle_name" />
          </p>
          <p>
            Last Name:
            <input type="text" v-model="contact.last_name" />
          </p>
          <p>
            Email:
            <input type="text" v-model="contact.email" />
          </p>
          <p>
            Phone Number:
            <input type="text" v-model="contact.phone_number" />
          </p>
          <p>
            Bio:
            <input type="text" v-model="contact.bio" />
          </p>
          <p>
            Address:
            <input type="text" v-model="contact.address" />
          </p>
          <button v-on:click="updateContact(contact)">Update info</button>
        </div>
      </div>
      <hr />
    </div>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      message: "Contacts App",
      contacts: [],
      newContactFirstName: "",
      newContactMiddleName: "",
      newContactLastName: "",
      newContactEmail: "",
      newContactPhoneNumber: "",
      newContactBio: "",
      newContactAddress: "",
      currentContact: "",
    };
  },
  created: function() {
    axios.get("/api/contacts").then(response => {
      console.log(response.data);
      this.contacts = response.data;
    });
  },
  methods: {
    addContact: function() {
      console.log("adding a new contact");
      let params = {
        first_name: this.newContactFirstName,
        middle_name: this.newContactMiddleName,
        last_name: this.newContactLastName,
        email: this.newContactEmail,
        phone_number: this.newContactPhoneNumber,
        bio: this.newContactBio,
        address: this.newContactAddress,
      };
      axios.post("/api/contacts", params).then(response => {
        this.contacts.push(response.data);
        this.newContactFirstName = "";
        this.newContactMiddleName = "";
        this.newContactLastName = "";
        this.newContactEmail = "";
        this.newContactPhoneNumber = "";
        this.newContactBio = "";
        this.newContactAddress = "";
      });
    },
    showContact: function(theContact) {
      this.currentContact = theContact;
    },
    updateContact: function(theContact) {
      console.log("Updating contact info...");
      let params = {
        first_name: theContact.first_name,
        middle_name: theContact.first_name,
        last_name: theContact.middle_name,
        email: theContact.last_name,
        phone_number: theContact.phone_number,
        bio: theContact.bio,
        address: theContact.address,
      };
      axios.patch("/api/contacts/" + theContact.id, params).then(response => {
        console.log(response.data);
        theContact = response.data;
      });
    },
    deleteContact: function(theContact) {
      let index = this.contacts.indexOf(theContact.id);
      axios.delete("/api/contacts/" + theContact.id).then(response => {
        console.log("Deleting contact...");
        let index = this.contacts.indexOf(theContact.id);
        this.contacts.splice(index, 1);
      });
    },
  },
};
</script>

