<template>
  <div class="panel panel-default">
    <div class="panel-body">
      {{{renderedMarkdown}}}
    </div>
  </div>
</template>

<script>
  import {gitHubUsername} from '../vuex/getters'

  export default {
    vuex: {
      getters: {
        gitHubUsername
      }
    },
    data: function () {
      return {
        renderedMarkdown: ''
      }
    },
    props: ['comment'],
    watch: {
      comment: function (comment) {
        this.fetchMarkdown()
      }
    },
    methods: {
      fetchMarkdown: function () {
        this.renderedMarkdown = ''
        if (this.comment.body) {
          this.$http.post('https://api.github.com/markdown', {
            "text": this.comment.body,
            "mode": "gfm",
            "context": this.gitHubUsername + "/" + this.gitHubUsername + ".github.io"
          }).then(function (response) {
            this.renderedMarkdown = response.data
          }, function (response) {
            console.log(response.data)
          })
        }
      }
    },
    ready: function () {
      this.fetchMarkdown()
    }
  }
</script>
