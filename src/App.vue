<template>
  <div id="app">
    <Typeahead/>
  </div>
</template>

<script>
import Typeahead from './components/Typeahead'
import Axios from 'axios'
import Vue from 'vue'

// Avoids polluting global scope; makes utility available to all Vue instances.
Vue.prototype.$http = Axios

export default {
  name: 'app',
  components: {
    Typeahead
  }
}
</script>
