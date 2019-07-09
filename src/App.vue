<template>
  <div id="app">
    <div>{{baseUrl}} {{workspace}}</div>
    <button @click="doQuery">Make Query</button>
  </div>
</template>

<script>

export default {
  name: 'app',
  data () {
    return {
      baseUrl: '',
      workspace: ''
    }
  },
  methods: {
    doQuery () {
      console.log(`${new Date().toISOString()} doing query...`)
      this.$lx.executeParentOriginXHR('POST', `${this.baseUrl}/services/webhooks/v1/subscriptions`)
        .then(res => {
          console.log('RES', res)
        })
        .catch(err => {
          console.error('err', err)
        })
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
</style>
