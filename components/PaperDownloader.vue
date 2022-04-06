<template>
  <div id="app">
    <div class="title"> 
      <h1>Paper Downloader</h1>
      <h3>This application was designed to download Paper easier.</h3>
    </div>

    <div class="content">

      <div class="version">
        <span>Version:</span>
        <template v-if="fetchedVersions">
          <select name="selector-version" id="selector-version" v-model="version">
            <option v-for="e in versions" :value="e"> {{ e }} </option>
          </select>
        </template>
        <template v-else>
          <span>Preparing...</span>
        </template>
      </div>

      <div class="build" v-if="isDefinedVersion">
        <span>Build: </span>
        <template v-if="fetchedBuilds">
          <select name="selector-build" id="selector-build" v-model="build">
            <option v-for="e in builds" :value="e"> {{ e }} </option>
          </select>
        </template>
        <template v-else>
          <span>Preparing...</span>
        </template>
      </div>

      <div class="result" v-show="isDefinedBuild">
        <p>Ready for download</p>
        <button id="btn-result-download" @click="download" :title="url">Download</button>
      </div>

    </div>
  </div>
</template>

<script>

export default{
  name: 'PaperDownloader',
  data() {
    return {
      fetchedVersions: false,
      fetchedBuilds: false,
      isDefinedVersion: false,
      isDefinedBuild: false,
      versions: [''],
      builds: [''],
      version: '',
      build: '',
      url: 'https://google.com/',
      VERSION_URL: 'https://papermc.io/api/v2/projects/paper'
    }
  },
  methods: {
    download (){
      window.location.href = this.url
    },
    getBuildURL(ver){
      return this.VERSION_URL+'/versions/' + ver
    },
    getDownloadURL(ver, build){
      return this.getBuildURL(ver)+'/builds/'+build+'/downloads/paper-'+ver+'-'+build+'.jar'
    }
  },
  watch: {
    version: function(newVer){
      this.isDefinedBuild = false
      this.fetchedBuilds = false

      fetch(this.getBuildURL(newVer)).then(res => res.json())
        .then( data => {
          this.builds = data.builds
          this.build = this.builds.slice().pop()

          this.fetchedBuilds = true
          this.isDefinedBuild = true
        })
    },
    build: function(newBuild){
      this.isDefinedBuild = false

      this.url = this.getDownloadURL(this.version, newBuild)

      this.isDefinedBuild = true
    }
  },
  mounted() {
    fetch(this.VERSION_URL).then( res => res.json())
    .then( data => {

      this.versions = data.versions
      this.version = this.versions.slice().pop() // Auto select latest version

      this.fetchedVersions = true
      this.isDefinedVersion = true
    })
  },
}

</script>