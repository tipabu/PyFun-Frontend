<template>
  <div v-if="title === ''">
    Loading ... 
  </div>
  <div v-else>
    <div id="description">
      <h1>{{ title }}</h1>
      <div id="image" v-if="image !== null">
        <img :src="image"/>
      </div>
      <br>
      <p v-for="des in description" :key="des">{{ des }}</p>
    </div>
    <div id="field">
      <form @submit.prevent="submit">
        <div class="form-group">
          <code v-html="processCode()"></code>
        </div>
        <div class="form-group">
          <button class="btn btn-primary" type="submit">Submit</button>
        </div>
      </form>
    </div>
    <div id="data" v-if="data !== null">
      <div style="padding-left: 15px;"><h3>Result: {{ data.result }}</h3></div>
      <div class="col">
        <h3>Stdout:</h3>
        <textarea v-model="data.stdout" cols=50 rows=15></textarea>
      </div>
      <div class="col">
        <h3>Stderr:</h3>
        <textarea v-model="data.stderr" cols=50 rows=15></textarea>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'stage',
  data: function () {
    return {
      levelName: '',
      stageName: '',
      title: '',
      image: null,
      description: [],
      fields: [],
      data: null,
      code: null
    }
  },
  mounted: function () {
    this.getStage()
  },
  watch: {
    $route (to, from) {
      if (to.name === 'Stage') {
        this.getStage()
      }
    }
  },
  methods: {
    getStage: function () {
      this.image = null
      this.data = null
      this.levelName = this.$route.params.level_name
      this.stageName = this.$route.params.stage_name
      this.$ajax({
        method: 'GET',
        url: this.$backend + '/stage/' + this.levelName + '/' + this.stageName
      }).then(response => {
        if (response.data.error !== undefined) {
          console.log(response.data.error)
        } else {
          this.title = response.data.data.title
          if (response.data.data.image !== undefined) {
            this.image = response.data.data.image
          }
          this.description = response.data.data.description
          this.code = response.data.data.code
          this.fields = response.data.data.fields
        }
      }).catch(error => {
        console.log(error)
      })
    },
    processCode: function () {
      var html = ''
      var fieldIndex = 0
      for (var index in this.code) {
        var code = this.code[index]
        var split = code.split(/_____/)
        for (var lineIndex in split) {
          html += split[lineIndex].split(/    /).join('&nbsp;&nbsp;&nbsp;&nbsp;')
          if ((split.length - 1) > lineIndex) {
            html += `<input name="${this.fields[fieldIndex]}" style="text-align: center;border: none;border-bottom: 1px solid #9e9e9e;"/>`
            fieldIndex++
          }
        }
        html += '</br>'
      }
      return html
    },
    submit: function (event) {
      this.data = null
      var elements = event.target.elements
      var postData = {}
      for (var key in this.fields) {
        postData[this.fields[key]] = elements[this.fields[key]].value
      }
      this.$ajax({
        method: 'POST',
        url: this.$backend + '/stage/' + this.levelName + '/' + this.stageName,
        headers: {
          'Content-Type': 'application/x-www-form-urlencoded'
        },
        data: postData
      }).then(response => {
        if (response.data.success !== undefined && response.data.success === true) {
          this.data = response.data.data
        } else {
        }
        console.log(response)
      }).catch(error => {
        console.log(error)
      })
    }
  }
}
</script>

<style scoped>
  div {
    text-align: left;
  }
  .col {
    width: calc(100% / 2);
    float: left;
  }
  textarea {
    border: none;
    resize: none;
    overflow: hidden;
    display: block;
    width: calc(100% - 4px);
  }
  label {
    display: block;
  }
</style>
