<script lang="ts">
import axios from 'axios'
import cfgObj from '../cfg/config.js'
import { registerRuntimeHelpers } from '@vue/compiler-core'

export default({
  data() {
    return {
      rows: JSON.parse(localStorage.getItem('favCards')),
      rowsForSearch: []
    }
  },
  mounted () {
    
    // axios
    //   .get('https://api.unsplash.com/photos?client_id='+cfgObj.idClient)
    //   .then((response) => {
    //     this.rows = response.data
    //     this.rowsForSearch = response.data
    //     window.arrtest = response.data
    //     //alert('DATA')
    //     console.log(response.data)
    //   })
  },
  methods: {
    openPost(e){
      console.log(e.currentTarget.id)
      //this.$router.push('/about?id='+e.currentTarget.id)
      window.location.href = '/about?id=' + e.currentTarget.id
    },
    searchPost(){
      const query = document.getElementById('searchInput').value

      this.rows = this.rowsForSearch.filter(result => result.user.name.includes(query));
    }
  }
})
</script>

<template>

<section class="py-1 text-center container main-search">
  <div class="row py-lg-3">
    <div class="col-lg-6 col-md-8 mx-auto">
      <h1 class="fw-light">Избранное</h1>
    </div>
  </div>
</section>

  <div class="album py-5 bg-light image-posts">
    <div class="container">

      <div class="row row-cols-1 row-cols-sm-2 row-cols-md-3 g-3" id="rowsMap">
        <div class="col" v-for="item in rows" :key="item">
          <div class="card shadow-sm" :id="item.id" @click="openPost($event)">
            <img class="image-post" :src="item.urls.full" alt="" loading="lazy">
            <div class="overlay">
              <div class="card-body">
                <h5 class="card-title text">{{item.user.name}}</h5>
                <p class="card-text text">{{item.user.bio}}</p>
                <p class="card-text text"><small class="text-muted text">Создано {{ new Date(item.created_at).toLocaleString() }}</small></p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
