<template>
  <div id="app">
    <header>
      <div class="header-items">
        <h1 class="title">FE: Gifts and Items</h1>
        <div class="search">
          <input type="text" name="search" id="search" ref="search"
            placeholder="Press '/' to focus"
            v-model="searchTerm"
            v-on:keyup.enter="completeSearchTerm=searchTerm"
          >
        </div>
      </div>
    </header>
    <main>
      <AllData :mode="mode" :searchTerm="completeSearchTerm" />
      <footer>
        <a class="link" href="colehollant.com">Cole Hollant</a>
      </footer>
    </main>
  </div>
</template>

<script>
import AllData from './components/AllData.vue'


export default {
  name: 'app',
  components: {
    AllData
  },
  data () {
    return {
      searchTerm: "",
      completeSearchTerm: "",
    }
  },
  methods: {
    focusSearch() {
      this.searchTerm = this.completeSearchTerm = "";
      this.$refs.search.focus();
    }
  },
  mounted () {
    window.addEventListener('keyup', (e)=>{
      if(e.keyCode == 191) this.focusSearch();
      return;
    })
  }
}
</script>

<style>
html, body {
  padding: 0;
  margin: 0;
}
html {
  background-color: #0e1720;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #63d69d;
  display: grid;
  grid-template-rows: 100px auto;
  max-height: 100vh;
}
header {
  background-color: #1c2e41;
  display: flex;
  align-items: center;
}
.header-items {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
  margin: auto;
  width: 100%;
  max-width: 1400px;
}
main {
  overflow-y: scroll;
  height: calc(100vh - 100px);
}
.title {
  font-size: 1.2rem;
  margin: 0 2rem 0.5rem;
}
input {
  outline: none;
  border: none;
  height: 32px;
  width: 200px;
  padding: 0 1rem;
  font-size: 1.25rem;
  border-radius: 16px;

}
.options-group {
  display: flex;
}
.mode-title {
  margin-top: 0;
  margin-bottom: 0.5rem;
}
.btn-option {
  cursor: pointer;
  margin: 0 0.5rem 0.5rem;
  background-color: rgb(58, 110, 110);
  width: 8ch;
  height: 4ch;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 1ch;
}
.btn-option:hover {
  background-color: rgb(44, 94, 94);
}
.link {
  text-decoration: none;
  margin: 1rem;
  display: inline-block;
  color: #63d69d;
}

@media screen and (min-width: 1000px) {
  .header-items {
    flex-direction: row;
  }
  .title {
    margin-bottom: 0;
    font-size: 1.5rem;
  }
}
</style>
