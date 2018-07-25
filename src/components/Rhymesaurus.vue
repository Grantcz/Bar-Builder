<template>
  <div class="rhymesaurus">
    <p>A Rhyming Thesaurus Built for the Local Struggle Rapper.</p>
    <form v-on:submit.prevent="findWords">
      <p>Find rhymes for <input type="text" v-model="rhyme"> related to <input type="text" v-model="phrase"> <button type="submit">Search</button></p>
    </form>
    <ul v-if="results && results.length > 0" class="results">
      <li v-for="item in results" class="item">
        <p><strong>{{ item.word }}</strong></p>
        <p>{{ item.score }}</p>
      </li>
    </ul>

    <div v-else-if="results && results.length === 0" class="no-results">
      <h2>No Words Found</h2>
      <p>Please adjust your search to find more words.</p>
    </div>

    <ul v-if="errors.length > 0" class="errors">
      <li v-for="error in errors">
        {{ error.message }}
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios'; 

export default {
  name: 'Rhymesaurus',
  data () {
    return {
      results: null,
      errors: [],
      phrase: '',
      rhyme: ''
    }
  },
  methods: {
    findWords: function() {
      axios.get('https://api.datamuse.com/words', {
        params: {
          ml: this.phrase,
          rel_rhy: this.rhyme 
        },
        headers:{
          'Accept': 'application/json',
          'Content-Type': 'application/json',
        } 
      })
      .then( response => {
        this.results = response.data;
      })
      .catch( error => {
        this.errors.push(error);
      })
    }
  }
}
</script>

<style scoped>
.rhymesaurus {
  font-size: 1.4rem;
}

input[type="text"]{
  border-top: none;
  border-left: none;
  border-right: none;
  border-bottom: 1px solid #000000;
  width: 300px;
  font-size: 1.4rem;
  color: #000000;
  font-weight: 300;
  background: rgba(0,0,0,0.02);
  padding: 0.5rem;
}
button{
  background: #000000;
  padding: 0.5rem;
  font-weight: 300;
  color: #cecece;
  border: none;
  cursor: pointer;
  font-size: 1.4rem;
}
h1, h2 {
  font-weight: normal;
}

ul.results {
  list-style-type: none;
  padding: 0;
}

.results li {
  display: inline-block;
  margin: 10px;
  border: solid 1px #000000;
  padding: 0.5rem;
  width: 200px;
  min-height: 100px;
  color: #000000;
  background: #cecece;
}
ul.errors {
  list-style-type: none;
}
.errors li {
  border: 1px solid red;
  color: red;
  padding: 0.5rem;
  margin: 10px 0;
}

</style>
