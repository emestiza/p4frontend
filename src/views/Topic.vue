<template>
  <div class="topic">
    <p>{{$route.query.info.username}}</p>

    <!-- Topic -->
    <b-field label="Topic" type="is-danger">
      <b-input type="text" v-model="topicName" maxlength="25"></b-input>
    </b-field>
    <b-field label="Description">
      <b-input maxlength="100" type="textarea" v-model="topicDescription"></b-input>
    </b-field>
    <b-field label="Subject">
      <b-select placeholder="Select a subject" v-model="subject">
        <option
          v-for="subject of subjects"
          v-bind:key="subject.id"
          v-bind:value="subject.id"
        >{{ subject.name }}</option>
      </b-select>
    </b-field>
    <b-button type="is-danger" @click="newTopic">New Topic</b-button>
    <br />
    <br />

    <b-field label="Edit Topic" type="is-danger">
      <b-input type="text" v-model="editTopicName" maxlength="25"></b-input>
    </b-field>
    <b-field label="Edit Description">
      <b-input maxlength="100" type="textarea" v-model="editTopicDescription"></b-input>
    </b-field>
    <b-field label="Edit Subject">
      <b-select placeholder="Select a subject" v-model="subjectEdit">
        <option
          v-for="subject of subjects"
          v-bind:key="subject.id"
          v-bind:value="subject.id"
        >{{ subject.name }}</option>
      </b-select>
    </b-field>
    <b-button type="is-danger" @click="editTopic" v-bind:id="editidTopic">Edit Topic</b-button>
    <br />
    <br />

    <ul>
      <!-- Topic v-bind:key="topic.id" v-bind:value="topic.id" -->
      <li v-for="topic of topics" :key=" 't-' + topic.id ">
        <b-message title="Topic" aria-close-label="Close message">
          <h1>{{ topic.name }}</h1>
          <h6>{{ topic.description }}</h6>
          <h6>{{ topic.subject_name }}</h6>
          <button
            v-bind:id="topic.id"
            class="button is-success is-outlined"
            @click="deleteTopic"
          >Delete</button>
          <button
            v-bind:id="topic.id"
            class="button is-success is-outlined"
            @click="() => {editSelectTopic(topic.id, topic.name, topic.subject, topic.description)}"
          >edit</button>
        </b-message>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "Topic",
  data: function() {
    return {
      subjects: [],
      topics: [],
      topicName: "",
      topicDescription: "",
      subject: "",
      subjectEdit: "",
      is_public: true,
      editTopicName: "",
      editTopicDescription: "",
      editidTopic: null
    };
  },
  created: function() {
    this.getSubjects();
    this.getTopics();
  },
  methods: {
    // Subject
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

    // Topic
    newTopic: function() {
      const { info, URL } = this.$route.query;

      fetch(`${URL}/api/topic/`, {
        method: "post",
        headers: {
          authorization: `JWT ${info.token}`,
          "Content-Type": "application/json"
        },
        body: JSON.stringify({
          name: this.topicName,
          description: this.topicDescription,
          subject: this.subject,
          is_public: this.is_public
        })
      }).then(() => {
        this.getTopics();
      });
    },
    getTopics: function() {
      const { info, URL } = this.$route.query;

      fetch(`${URL}/api/topic/`, {
        method: "get",
        headers: {
          authorization: `JWT ${info.token}`
        }
      })
        .then(response => response.json())
        .then(data => {
          console.log(data);
          this.topics = data.results;
        });
    },
    deleteTopic: function(event) {
      const { info, URL } = this.$route.query;
      const id = event.target.id;

      fetch(`${URL}/api/topic/${id}/`, {
        method: "delete",
        headers: {
          authorization: `JWT ${info.token}`
        }
      }).then(() => {
        this.getTopics();
      });
    },
    editSelectTopic: function(id, name, subject, description) {
      this.editidTopic = id;
      this.editTopicName = name;
      this.editTopicDescription = description;
      this.subjectEdit = subject;
    },
    editTopic: function() {
      const { info, URL } = this.$route.query;
      const id = this.editidTopic;

      fetch(`${URL}/api/topic/${id}/`, {
        method: "put",
        headers: {
          authorization: `JWT ${info.token}`,
          "Content-Type": "application/json"
        },
        body: JSON.stringify({
          name: this.editTopicName,
          description: this.editTopicDescription,
          subject: this.subjectEdit
        })
      }).then(() => {
        this.getTopics();
      });
    }
  }
};
</script>

<style>
div.topic {
  width: 25%;
  min-width: 300px;
  margin: 10px auto;
}
</style>

