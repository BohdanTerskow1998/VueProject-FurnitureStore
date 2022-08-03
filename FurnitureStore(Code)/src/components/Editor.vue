<script>

  import {marked} from 'marked'
  import {debounce} from 'lodash-es'

  export default {
    data() {
      return {
        text: "# html",
      }
    },
    computed: {
      output() {
        return marked(this.text)
      }
    },
    methods: {
      update: debounce(function(e) {
        return this.text = e.target.value
      })
    },
    watch: {
      text(value) {
        console.log(value)
      }
    }
  }

</script>

<template>

  <div class="editor"> 
    <textarea class="input" :value="text" @input="update"></textarea>
    <div class="output" v-html="output"></div>
  </div>

</template>

<style>

  .editor {
    height: 100vh;
    display: flex;
  }

  .input, .output {
    width: 50%;
    height: 100%;
    padding: 0px 20px;
    box-sizing: border-box;
    overflow: auto;
  }

  .input {
    resize: none;
    outline: none;
    border: none;
    border-right: 1px solid black;
    font-size: 25px;
    padding: 20px;
  }

</style>