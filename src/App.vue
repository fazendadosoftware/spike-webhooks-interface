<template>
  <div id="app">
    <div>{{baseUrl}}</div>
    <div>{{workspace}}</div>
    <div class="button-container">
      <button :disabled="requestOngoing" @click="doQuery()">Query Webhooks API (GET)</button>
      <button :disabled="requestOngoing" @click="doQuery('POST')">Query Webhooks API (POST)</button>
    </div>
    <pre>{{response}}</pre>
  </div>
</template>

<script>

export default {
  name: 'app',
  data () {
    return {
      baseUrl: '',
      workspace: '',
      response: {},
      requestOngoing: false
    }
  },
  methods: {
    doQuery (method = 'GET') {
      if (this.requestOngoing) return
      this.requestOngoing = true
      this.$lx.executeParentOriginXHR(method, `${this.baseUrl}/services/webhooks/v1/subscriptions`)
        .then(res => {
          this.response = res
          console.log('RES', res)
          this.requestOngoing = false
        })
        .catch(err => {
          this.request = err
          console.error('err', err)
          this.requestOngoing = false
        })
    }
  },
  watch: {
    requestOngoing (state) {
      if (state) this.$lx.showSpinner()
      else this.$lx.hideSpinner()
    }
  },
  created () {
    this.$lx.init()
      .then(setup => {
        const { settings } = setup
        const { baseUrl } = settings
        const path = baseUrl.split('/')
        this.baseUrl = path.slice(0, path.length - 1).join('/')
        this.workspace = path[path.length - 1]
        console.log('report setup', setup)
        this.$lx.ready({})
      })
  }
}
</script>
<style lang="stylus">
#app
  font-family 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color #2c3e50
  margin-top 60px
.button-container
  background red
</style>
