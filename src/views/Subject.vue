<template>
  <div class="subject">
    <p>{{$route.query.info.username}}</p>

    <!-- Subject -->
    <b-field label="Subject" type="is-danger">
      <b-input type="text" v-model="subjectName" maxlength="25"></b-input>
    </b-field>
    <b-field label="Description">
      <b-input maxlength="100" type="textarea" v-model="description"></b-input>
    </b-field>
    <b-button type="is-danger" @click="newSubject">New Subject</b-button>
    <br />
    <br />

    <b-field label="Edit Subject" type="is-danger">
      <b-input type="text" v-model="editSubjectName" maxlength="25"></b-input>
    </b-field>
    <b-field label="Edit Description">
      <b-input maxlength="100" type="textarea" v-model="editSubjectDescription"></b-input>
    </b-field>
    <b-button type="is-danger" @click="editSubject" v-bind:id="editid">Edit Subject</b-button>
    <br />
    <br />

    <ul>
      <!-- Subject -->
      <li v-for="subject of subjects" v-bind:key="subject.id">
        <b-message title="Subject" aria-close-label="Close message">
          <h1>{{ subject.name }}</h1>
          <h6>{{ subject.description }}</h6>
          <button
            v-bind:id="subject.id"
            class="button is-success is-outlined"
            @click="deleteSubject"
          >Delete</button>
          <button
            v-bind:id="subject.id"
            class="button is-success is-outlined"
            @click="() => {editSelect(subject.id, subject.name, subject.description)}"
          >edit</button>
        </b-message>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "Subject",
  data: function() {
    return {
      subjects: [],
      subjectName: "",
      description: "",
      editSubjectName: "",
      editSubjectDescription: "",
      editid: null
    };
  },
  created: function() {
    this.getSubjects();
  },
  methods: {
    // Subject
    newSubject: function() {
      const { info, URL } = this.$route.query;

      fetch(`${URL}/api/subject/`, {
        method: "post",
        headers: {
          authorization: `JWT ${info.token}`,
          "Content-Type": "application/json"
        },
        body: JSON.stringify({
          name: this.subjectName,
          description: this.description
        })
      }).then(() => {
        this.getSubjects();
      });
    },
    getSubjects: function() {
      const { info, URL } = this.$route.query;

      fetch(`${URL}/api/subject/`, {
        method: "get",
        headers: {
          authorization: `JWT ${info.token}`
        }
      })
        .then(response => response.json())
        .then(data => {
          console.log(data);
          this.subjects = data.results;
        });
    },
    deleteSubject: function(event) {
      const { info, URL } = this.$route.query;
      const id = event.target.id;

      fetch(`${URL}/api/subject/${id}/`, {
        method: "delete",
        headers: {
          authorization: `JWT ${info.token}`
        }
      }).then(() => {
        this.getSubjects();
      });
    },
    editSelect: function(id, name, description) {
      this.editid = id;
      this.editSubjectName = name;
      this.editSubjectDescription = description;
    },
    editSubject: function() {
      const { info, URL } = this.$route.query;
      const id = this.editid;

      fetch(`${URL}/api/subject/${id}/`, {
        method: "put",
        headers: {
          authorization: `JWT ${info.token}`,
          "Content-Type": "application/json"
        },
        body: JSON.stringify({
          name: this.editSubjectName,
          description: this.editSubjectDescription
        })
      }).then(() => {
        this.getSubjects();
      });
    }
  }
};
</script>

<style>
div.subject {
  width: 25%;
  min-width: 300px;
  margin: 10px auto;
}
</style>

