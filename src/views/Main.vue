<template>
  <div class="main">
    <b-field label="New Subject" type="is-danger" message="Enter a new Subject here">
      <b-input type="text" v-model="message" maxlength="25"> </b-input>
    </b-field>
    <b-button type="is-danger" @click="newSubject">New Subject</b-button><br /><br />

        <b-field label="New Subject" type="is-danger" message="Edit a Subject Here">
      <b-input type="text" v-model="editmessage" maxlength="25"> </b-input>
    </b-field>
    <b-button type="is-danger" @click="editSubject" v-bind:id="editid">Edit Subject</b-button><br /><br />
    <ul>
      <li v-for="subject of subjects" v-bind:key="subject.id">
        <b-message title="Default" aria-close-label="Close message">
          {{ subject.name }}
          <button
            v-bind:id="subject.id"
            class="button is-success is-outlined"
            @click="deleteSubject"
          >
            Delete
          </button>
          <button
            v-bind:id="subject.id"
            class="button is-success is-outlined"
            @click="() => {editSelect(subject.id, subject.name, subject.description)}"
          >
            edit
          </button>
        </b-message>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "Main",
  data: function() {
    return {
      subjects: [],
      message: "",
      editmessage: "",
      editid: null
    };
  },
  created: function() {
    this.getSubjects();
  },
  methods: {
    newSubject: function() {
      const { tokens, URL } = this.$route.query;

      fetch(`${URL}/api/subject/`, {
        method: "post",
        headers: {
          authorization: `JWT ${tokens.token}`,
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ message: this.name }),
      }).then(() => {
        this.getSubjects();
      });
    },
    getSubjects: function() {
      const { tokens, URL } = this.$route.query;

      fetch(`${URL}/api/subject/`, {
        method: "get",
        headers: {
          authorization: `JWT ${tokens.token}`,
        },
      })
        .then((response) => response.json())
        .then((data) => {
          console.log(data);
          this.subjects = data.results;
        });
    },
    deleteSubject: function(event) {
      const { tokens, URL } = this.$route.query;
      const id = event.target.id;

      fetch(`${URL}/api/subject/${id}/`, {
        method: "delete",
        headers: {
          authorization: `JWT ${tokens.token}`,
        },
      }).then(() => {
        this.getSubjects();
      });
    },
    editSelect: function(id, content){
      this.editid = id
      this.editmessage = content
    },
    editSubject: function() {
      const { tokens, URL } = this.$route.query;
      const id = this.editid;

      fetch(`${URL}/api/subject/${id}/`, {
        method: "put",
        headers: {
          authorization: `JWT ${tokens.token}`,
          "Content-Type": "application/json"
        },
        body: JSON.stringify({message: this.editmessage})
      }).then(() => {
        this.getSubjects();
      });
    }
  },
};
</script>

<style>
div.main {
  width: 70%;
  min-width: 300px;
  margin: 10px auto;
}
</style>


