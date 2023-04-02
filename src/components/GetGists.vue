<template>
  <div>
    <h1>Gists for {{ username }}</h1>
    <input name="username" v-model="username" placeholder="github username -> click" />
   <button v-on:click="getInfo()">click</button>
    <div class="container">
      <div class="row">
      <div class="col-md-3 show" v-show="display">
        <img alt="GH Avatar Image" :src="image">
        <h3> {{ name }} @ {{ location }}</h3>
        <br>
        <h4>{{ bio }}</h4>
        <a :href="url">GitHub profile</a>

      </div>
      <div class="col-md-1"></div>
      <div class="col-md-8">

        <table class="table">
  <thead >
    <tr>
      <th scope="col">gist name</th>
      <th scope="col">created at</th>
      <th scope="col">language</th>
    </tr>
  </thead>
  <tbody v-for="gist in gists" :key="gist.id">
    <tr>
      <td><a :href="gist.html_url">{{ gist.description }}</a></td>
      <td>{{ gist.created_at.substr(0,10) }}</td>
      <td>{{ gist.files.language }}</td>
    </tr>
  </tbody>
</table>


      </div>
    </div>
  </div>
  </div>

</template>

<script>
import { Octokit } from '@octokit/rest';


export default {

  data() {
    return {
      username: "",
      gists: [],
      userdata: [],
      image:"",
      url:"",
      name:"",
      location:"",
      bio:""
    };
  },

  methods:{
    getInfo(){
      this.display = true;
      const octokit = new Octokit({
      auth: '',
    });
      octokit.gists.listForUser({
      username: this.username,
    })
      .then((response) => {
        this.gists = response.data;
        console.log(response);
        this.image = this.gists[0].owner.avatar_url;
        this.url = this.gists[0].owner.html_url;
      })
      .catch((error) => {
        console.error(error);
      });

      octokit.rest.users.getByUsername({
      username:this.username, 
    }).
      then((response) => {
        this.userdata = response.data;
        console.log(response);
        this.name = this.userdata.name;
        this.location = this.userdata.location;
        this.bio = this.userdata.bio;

      })
      .catch((error) => {
        console.error(error);
      });

    },

  }
};
</script>

