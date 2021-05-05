<template>
  <div id="app">
    <div class="nav">
      <h3>Playlist Generator</h3>
    </div>

    <div class="sidebar">
      <div class="sidebar-container">
        <input class="search" placeholder="Search for artists...">
        <div class="artists-container">
          <div class="artist bg-red"></div>
          <div class="artist bg-green"></div>
          <div class="artist bg-blue"></div>
          <div class="artist bg-orange"></div>
          <div class="artist bg-red"></div>
          <div class="artist bg-green"></div>
          <div class="artist bg-blue"></div>
          <div class="artist bg-orange"></div>
          <div class="artist bg-red"></div>
          <div class="artist bg-green"></div>
          <div class="artist bg-blue"></div>
          <div class="artist bg-orange"></div>
        </div>
        <h3>Playlist generation options</h3>
        <form class="generation-options" action="">
          <div class="checkboxes">
            <label for="x"><input type="checkbox" id="x" /> <span>Related artists</span></label><br>
            <label for="x"><input type="checkbox" id="x" /> <span>Older Stuff</span></label><br>
            <label for="y"><input type="checkbox" id="y" /> <span>Newer Stuff</span></label><br>
            <label for="z"><input type="checkbox" id="z" /> <span>Artists from genres</span></label><br>
          </div>
        </form>
        <button class="generate-btn">Generate Playlist</button>
      </div>
    </div>

    <div class="main">
    </div>


  </div>
</template>

<script>



export default {  
  name: 'App',
  components: {

  },
    data: function () {
    return {
      client_id: '817ff5ae4f5e4387b0d1ee923ad84792',
      scopes: 'user-top-read',
      redirect_uri: 'http://localhost:8080',
      me: null,
      token: '',
      artists: [],
      authorized: false,
      favArtists: [],
    }
  },
  methods: {
    onClickChild(value){
      this.favArtists.push(value);
    },
    addArtist(item){
      this.favArtists.push(item);
      this.results = null;
      this.query = '';
    },
    logon(){
      let popup = window.open(`https://accounts.spotify.com/authorize?client_id=${this.client_id}&response_type=token&redirect_uri=${this.redirect_uri}&scope=${this.scopes}&show_dialog=true`, 'Login with Spotify', 'width=800,height=600')
      
      window.spotifyCallback = (payload) => {
        popup.close()
        this.token = payload;
        fetch('https://api.spotify.com/v1/me', {
          headers: {
            'Authorization': `Bearer ${payload}`
          }
        }).then(response => {
          return response.json()
        }).then(data => {
          this.me = data
          if(this.me != null){
            this.authorized = true;
          }
        })
      }
    }
    },
  mounted() {
    this.token = window.location.hash.substr(1).split('&')[0].split("=")[1]
    
    if (this.token) {
      // alert(this.token)
      
      window.opener.spotifyCallback(this.token)
    }
  },

  props: {
    msg: String
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #000000;
  margin-top: 60px;
  display: grid;
  grid-template-rows: 50px auto;
  grid-template-columns: 25% 1fr;
  padding: 0;
  margin: 0;
  height: 100vh;
}

.nav {
  grid-row-start: 1;
  grid-row-end: 2;
  grid-column-start: 1;
  grid-column-end: 3;
  background-color: rgb(27, 27, 27);
  width: 100%;
  height: 100%;
}

.nav h3 {
  color: white;
  vertical-align: middle;
  margin-left: 25px;
  
}

.sidebar {
  grid-row-start: 2;
  grid-row-end: 3;
  grid-column-start: 1;
  grid-column-end: 2;
  height: 100%;
  width: 100%;
  background-color: #F9FBFE;
  border-right: 1px solid #D8D8D8;
}

.sidebar-container {
  margin: 25px;
}

h3 {
  color: rgb(32, 32, 32);
}

.search {
  width: 100%;
  margin-bottom: 40px;
}



.artists-container {
  height: 500px;
  width: 100%;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: space-between;
  row-gap: 2em;
  overflow: auto;
}

.artist {
  background-color: #cecece;
  height: 100px;
  min-width: 125px;
}

body {
  background-color: #ffffff;
  margin: 0;
  
}



.checkboxes label {
  display: inline-block;
  padding-right: 15px;
  white-space: nowrap;
}

.checkboxes label span {
  vertical-align: middle;
}
.checkboxes input {
  margin-bottom: 20px;
}

.generate-btn {
  background-color:#9B51E0;
  border: none;
  border-radius: 15px;
  width: 100%;
  color: white;
  padding: 10px;
}

.bg-red {
  background-color: #EB5757;
}

.bg-orange {
  background-color: #F2994A;
}

.bg-green {
  background-color: #6FCF97;
}

.bg-blue {
  background-color: #56CCF2;
}

</style>
