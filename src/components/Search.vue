<template>
<div class="search">
    <h1>Search</h1>
    <input class="search" v-model="query" @input="debouncedGetSearch">
    <button @click="search">Search</button>
    <div class="dropdown" v-if="results">
        <ul v-if="results">
        <li v-bind:key="item.name" v-for="item in results.data.artists.items" @click="addArtist(item)">
            {{item.name}}
        </li>
        </ul>
    </div>
</div>  
</template>

<script>
import _ from 'lodash'
import axios from 'axios'

export default {
name: 'Search',
data: function () {
return {
    results: null,
    query: '',
    }
  },
methods: {
    addArtist(item){
      this.results = null;
      this.$emit('clicked',item);
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

.search {
    grid-row-start: 1;
    grid-column-start: 1;
}
</style>