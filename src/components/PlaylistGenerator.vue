<template>
<div class="generator">
    <h1>Generator</h1>
    <button>Create Playlist</button>
</div>  
</template>

<script>
import _ from 'lodash'
import axios from 'axios'

export default {
name: 'PlaylistGenerator',
data: function () {
return {
    results: null,
    query: '',
    }
  },
methods: {
    addArtist(item){
      this.favArtists.push(item);
      this.results = null;
      this.query = '';
    },
    search(){
      if(this.query == ''){
          this.results = null;
      }  

      if(this.query != '' || this.query != null){
        axios.get( `https://api.spotify.com/v1/search?q=${this.query}&type=artist`, {
        headers: {
            Authorization: 'Bearer ' + this.token //the token is a variable which holds the token
        }
        }).then(response => (this.results = response));
      }
    },
},
created: function(){
this.debouncedGetSearch = _.debounce(this.search, 250);
},
props: {
    token: String,
}
}
</script>

<style>
.dropdown {
  z-index: 1;
}
.generator {
    grid-column-start: 3;
    background-color: white;
    height: 100vh;
}


</style>