<template>
<div>
  <h1 class="page-header">Accounts</h1>

  <div v-if="loading" class="loading">
    Loading...
  </div>

  <div v-if="error" class="error">
    {{ error }}
  </div>

  <table v-if="post">
    <thead>
    <tr>
        <th>Account ID</th>
        <th>Account Name</th>
        <th>Account Number</th>
        <th>Account Type</th>
    </tr>
    </thead>
    <tbody>
    <tr v-for="account in post" :key="account">
        <td>{{account.id}}</td>
        <td>{{account.name}}</td>
        <td>{{account.number}}</td>
        <td>{{account.type}}</td>
    </tr>
    </tbody>
  </table>
</div>
</template>

<script charset="utf-8">
export default {
  data () {
    return {
      loading: false,
      post: null,
      error: null
    }
  },
  created () {
    this.fetchData()
  },
  watch: {
    $route: "fetchData"
  },
  methods: {
    fetchData: function() {
      this.error = this.post = null
      this.loading = true
      // fetch post data from an external API endpoint
      fetch("http://localhost:3000/accounts/").then(async response => {
        const data = await response.json();

        // check for error response
        if (!response.ok) {
          // get error message from body or default to response statusText
          const error = (data && data.message) || response.statusText;
          return Promise.reject(error);
        }
        this.post = data;
      })
      .catch(error => {
        this.error = error;
        console.error("There was an error!", error);
      });
    }
  }
};
</script>