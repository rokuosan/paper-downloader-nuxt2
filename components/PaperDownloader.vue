<template>
  <div id="app" class="font-questrial md:w-2/4 mx-auto">

    <div class="title my-8 border-l-8 border-[#52616B] px-5 py-4 mx-2"> 
      <h1 class="md:text-6xl text-5xl">Paper Downloader</h1>
      <h3 class="md:text-2xl text-xl italic">This application was designed to download Paper easier.</h3>
    </div>

    <div class="content text-2xl py-5 w-5/6 mx-auto grid grid-cols-1 gap-10">

      <div class="version flex items-center gap-3">
        <span>Version:</span>
        <template v-if="fetchedVersions">
          <select name="selector-version" id="selector-version" v-model="version" class="form-select rounded-md border-[#52616B]">
            <option v-for="e in versions" :value="e"> {{ e }} </option>
          </select>
        </template>
        <template v-else>
          <div class="inline-flex ml-3 gap-4">
            <div class="animate-ping h-2 w-2 bg-[#52616B] rounded-full"></div>
            <div class="animate-ping h-2 w-2 bg-[#52616B] rounded-full"></div>
            <div class="animate-ping h-2 w-2 bg-[#52616B] rounded-full"></div>
          </div>
        </template>
      </div>

      <div class="build flex items-center gap-3" v-if="isDefinedVersion">
        <span>Build: </span>
        <template v-if="fetchedBuilds">
          <select name="selector-build" id="selector-build" v-model="build" class="form-select rounded-md border-[#52616B]">
            <option v-for="e in builds" :value="e"> {{ e }} </option>
          </select>
        </template>
        <template v-else>
          <div class="inline-flex ml-3 gap-4">
            <div class="animate-ping h-2 w-2 bg-[#52616B] rounded-full"></div>
            <div class="animate-ping h-2 w-2 bg-[#52616B] rounded-full"></div>
            <div class="animate-ping h-2 w-2 bg-[#52616B] rounded-full"></div>
          </div>
        </template>
      </div>

      <div class="result my-5" v-show="isDefinedBuild">
        <div>
          <div class="h-0.5 w-full bg-black"></div>
        </div>
        <div class="my-5 flex flex-col gap-5">
          <p class="text-center">Ready for download</p>
          <button id="btn-result-download" @click="download" :title="url" class="mx-auto w-4/6 transition duration-200 ease-linear border-2 border-[#52616B] py-2 px-4 hover:bg-[#52616B] hover:text-[#F0F5F9]">Download</button>
        </div>
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