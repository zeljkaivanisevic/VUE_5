<template>

      <div class="card text-center">
        <div class="card-header text-left">
          {{ info.email }}
        </div>
        <div class="card-body">
          <img @click="changeImage" class="card-img-top" :src="info.url" alt=" slika">
        </div>
        <div class="card-footer text-left">
          {{ postedFromNow }}
        </div>
      </div>

</template>

<script>
import moment from 'moment'
export default {
  props: [ "info" ],
  methods: {
    changeImage() {
      this.info.time = 'Fetching...'
      fetch("https://source.unsplash.com/1600x900/?nature,water").then(response => {
        this.info.url = response.url
        this.info.time = 'Done.'
      })
    }
  },
  computed:{
    postedFromNow(){
      return moment(this.info.posted_at).fromNow();
    }
  }
}
</script>
