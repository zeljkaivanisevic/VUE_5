  
<template>
  <div id="app">
      <div class="bg-white">
        <div class="container">
          <div class="row">
            <div class="col">
              <nav class="navbar navbar-expand-lg navbar-light mb-2">
                <router-link class="navbar-brand" :to="{name: 'home'}">
                <svg aria-label="Instagram" class="_8-yf5 " fill="#262626" height="24" viewBox="0 0 48 48" width="34"><path d="M13.86.13A17 17 0 008 1.26 11 11 0 003.8 4 12.22 12.22 0 001 8.28 18 18 0 00-.11 14.1c-.13 2.55-.13 3.38-.13 9.9s0 7.32.13 9.9A18 18 0 001 39.72 11.43 11.43 0 003.8 44 12.17 12.17 0 008 46.74a17.75 17.75 0 005.82 1.13c2.55.13 3.38.13 9.9.13s7.32 0 9.9-.13a17.82 17.82 0 005.83-1.13A11.4 11.4 0 0043.72 44a11.94 11.94 0 002.78-4.24 17.7 17.7 0 001.13-5.82c.13-2.55.13-3.38.13-9.9s0-7.32-.13-9.9a17 17 0 00-1.13-5.86A11.31 11.31 0 0043.72 4a12.13 12.13 0 00-4.23-2.78A17.82 17.82 0 0033.66.13C31.11 0 30.28 0 23.76 0s-7.31 0-9.9.13m.2 43.37a13.17 13.17 0 01-4.47-.83 7.25 7.25 0 01-2.74-1.79 7.25 7.25 0 01-1.79-2.74 13.23 13.23 0 01-.83-4.47c-.1-2.52-.13-3.28-.13-9.7s0-7.15.13-9.7a12.78 12.78 0 01.83-4.44 7.37 7.37 0 011.79-2.75A7.35 7.35 0 019.59 5.3a13.17 13.17 0 014.47-.83c2.52-.1 3.28-.13 9.7-.13s7.15 0 9.7.13a12.78 12.78 0 014.44.83 7.82 7.82 0 014.53 4.53 13.12 13.12 0 01.83 4.44c.13 2.51.13 3.27.13 9.7s0 7.15-.13 9.7a13.23 13.23 0 01-.83 4.47 7.9 7.9 0 01-4.53 4.53 13 13 0 01-4.44.83c-2.51.1-3.28.13-9.7.13s-7.15 0-9.7-.13m19.63-32.34a2.88 2.88 0 102.88-2.88 2.89 2.89 0 00-2.88 2.88M11.45 24a12.32 12.32 0 1012.31-12.35A12.33 12.33 0 0011.45 24m4.33 0a8 8 0 118 8 8 8 0 01-8-8"></path></svg>
                <img src="/images/logo.png" width="103" height="29" >
              </router-link>
              <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
              </button>
            
              <div class="collapse navbar-collapse" id="navbarSupportedContent">
                <form class="form-inline my-2 my-lg-0 mr-auto ml-5">
                  <input v-model="searchTerm" class="form-control mr-sm-2" type="search" placeholder="Trazi nesto" aria-label="Search">
                </form>
                <router-link v-if="!authenticated" class="btn btn-info my-2 my-sm-0 mr-2" to="/login">Login</router-link>
                <span v-if="authenticated">
                  {{ userEmail }}
                  <a @click="logout" class="btn btn-info my-2 my-sm-0 mr-2" href="#">Logout</a>
                </span>
                <router-link v-if="!authenticated" class="btn btn-outline my-2 my-sm-0 mr-2" to="/signup">Signup</router-link>
            </div>
          </nav>
        </div>
      </div>
    </div>
    </div>

      <div class="container">

        <router-view/>

    </div>

    <div id="footer" class="text-center mt-5">Ovo je footer. (c) 2019 mi svi skupa.</div>
  </div>
</template>

<script type="text/javascript">
import store from '@/store.js'
export default {
  data () {
    return store;
  },
  methods: {


    
    logout() {
      firebase.auth().signOut()
    }
  },
  mounted () {
    firebase.auth().onAuthStateChanged(user => {
      if (user) {
        console.log("User is logged in with email " + user.email)
        this.authenticated = true
        this.userEmail = user.email
        if (this.$route.name !== 'home')
          this.$router.push({name: 'home'}).catch(err => console.log(err))
      }
      else {
        console.log("User is not logged in")
        this.authenticated = false
        if (this.$route.name !== 'login')
          this.$router.push({name: 'login'}).catch(err => console.log(err))
      }
    });
    db.collection("posts").orderBy("posted_at").limit(10)
      .onSnapshot(snapshot => {
        snapshot.docChanges().forEach(change => {
          const data = change.doc.data()
          if (change.type !== "added") return
            const card = {id: change.doc.id, url: data.url, email: data.email, title: this.userEmail, posted_at: data.posted_at}
            this.cards.unshift(card)
        });
    });
  }



  
}
</script>

<style lang="scss">
body {
  background-color: rgba(var(--b3f,250,250,250),1)
}
.bg-white {
  background-color: white;
  border-bottom: #ccc 1px solid;
  margin-bottom: 20px;
  padding: 10px;
}
nav.navbar {
  background-color: white;
  a {
    svg {
      border-right: #aaa 1px solid;
      margin-right: 10px;
      padding-right: 10px;
    }
    img {
      position: relative;
      top: 3px;
    }
  }
}
</style>
