<template>
  <div>
    <h3 class="my-2">Poor Man's Twitter</h3>
    <h4 class="my-4">Add New Tweet</h4>
    <b-form @submit="onSubmit">
      <b-form-group
        id="name-group"
        label="Name:"
        label-for="name"
      >
        <b-form-input
          id="name"
          v-model="form.name"
          type="text"
          placeholder="Enter your name"
          required
        ></b-form-input>
      </b-form-group>

      <b-form-group 
        id="message-group" 
        label="Message" 
        label-for="message">
        <b-form-textarea
          id="message"
          v-model="form.message"
          :state="form.message.length <= 50"
          placeholder="Enter message up to 50 characters"
          required
          aria-invalid="true"
        ></b-form-textarea>
        <b-form-text id="message-help-block" v-if="maxLimitError" class="text-danger">
          {{ maxLimitError }}
        </b-form-text>
      </b-form-group>
      <b-button type="submit" variant="primary">Submit</b-button>
    </b-form>
    <h4 class="my-4">Tweet List</h4>
    <b-table striped hover 
      :items="tweets"
      :fields="fields"
      :sort-by.sync="sortBy"
     :sort-desc.sync="sortDesc"
     sort-icon-left
    ></b-table>
  </div>
</template>

<script>
  import axios from 'axios';

  export default {
    data() {
      return {
        form: {
          name: '',
          message: '',
        },
        apiUrl: "https://twiterapplications.herokuapp.com",
        sortBy: 'time',
        sortDesc: true,
        fields: [
          { key: 'name', sortable: true },
          { key: 'time', sortable: true },
          { key: 'message', sortable: false }
        ],
        tweets: [],
        maxLimitError: null
      }
    },
    created() {
      this.getTweets();
    },
    methods: {
      async onSubmit(event) {
        try {
          event.preventDefault()
          console.log(event)
          if(this.form.message.length > 50) {
            this.maxLimitError = " your tweet exceeded to 50 characters.";
            return;
          }
          await axios.post(`${this.apiUrl}/create_tweet/`, this.form);
          this.getTweets();
          this.form = { name: "", message: ""};
        } catch (e) {
          console.log(e);
          alert("There is some error please try again.");
        }
      },

      async getTweets() {
        const response = await axios.get(`${this.apiUrl}/get_tweet_list/`);
        this.tweets = response.data;
        this.tweets.map(tweet => tweet.time = new Date(tweet.time).toLocaleString());
      }
    }
  }
</script>

<style scoped>
  #message-help-block {
    color: #dc3545 !important;
  }
</style>