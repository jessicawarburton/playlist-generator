<template>
<div>
    <div class="artists" v-if="artists" >
      <div class="artist" v-bind:key="artist.id" v-for="artist in artists.slice().reverse()">
          <img class="responsive" :src="artist.images[0].url" alt="">
          <p>{{artist.name}}</p>
      </div>
      </div>
</div>  
</template>

<script>

export default {
name: 'Artists',
  props: {
    artists: Array
  }
}
</script>

<style>
.artists{
    grid-column-start: 2;
    display: inline-flex;
  /* Then we define the flow direction 
     and if we allow the items to wrap 
   * Remember this is the same as:
   * flex-direction: row;
   * flex-wrap: wrap;
   */
  flex-flow: row wrap;
  justify-content: space-around;
  padding: 0;
  margin: 0;
}

.artist {
  flex: 2;
  flex-flow: row wrap;
  justify-content: flex-start;
  align-content: flex-start;
  border: 1px solid #cecece;
  height: auto;
  margin: 5px;
  min-width: 250px;
  max-width: 250px;
}

.artist p {
  padding: 5px;
  text-align: center;
  color: white;
}

.responsive {
  width: 100%;
  height: 250px;
}

</style>