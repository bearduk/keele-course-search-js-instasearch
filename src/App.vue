<template>
  <div class="container">
      <div id="search-box">
        <!-- SearchBox widget will appear here -->
      </div>

      <div id="hits">
        <!-- Hits widget will appear here -->
      </div>
  </div> <!-- .container -->
</template>

<script>
import instantsearch from 'instantsearch.js';

export default {
  name: 'app',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  },
  mounted(){

    const search = instantsearch({
          appId: 'HT7VYJG3KU',
          apiKey: 'd37bbf3291b226676c9f3f1937e865d3',
          indexName: 'dev_COURSES',
          urlSync: true
        });

    // initialize SearchBox
    search.addWidget(
      instantsearch.widgets.searchBox({
        container: '#search-box',
        placeholder: 'Search for products'
      })
    );

    // initialize hits widget
    search.addWidget(
      instantsearch.widgets.hits({
        container: '#hits'
      })
    );
  
    search.start();
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
