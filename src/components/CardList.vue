<script>
import axios from 'axios'
import cfgObj from '../cfg/config.js'
import { registerRuntimeHelpers } from '@vue/compiler-core'
import { RouterLink } from 'vue-router'

export default({
  data() {
    return {
      rows: [],
      rowsForSearch: []
    }
  },
  mounted () {
    
    axios
      .get(cfgObj.url+'/photos?client_id='+cfgObj.idClient)
      .then((response) => {
        this.rows = response.data
        this.rowsForSearch = response.data
        window.arrForSearch = response.data
        //alert('DATA')
        console.log(response.data)
      })
  },
  methods: {
    openPost(e){
      console.log(e.currentTarget.id)
      this.$router.push('/about?id='+e.currentTarget.id)
      //window.location.href = '/about?id=' + e.currentTarget.id
    },
    searchPost(){
      const query = document.getElementById('searchInput').value.toLowerCase();
      
      if(query != ""){
        axios
        .get(cfgObj.url + '/search/photos?client_id=' + cfgObj.idClient + '&page=1&query=' + query)
        .then((response) => {
          this.rows = response.data.results
          //alert('DATA')
          console.log(response.data)
        })
      }else{
        this.rows = this.rowsForSearch
      }
        // CLIENT SEARCH
        // this.rows = window.arrForSearch.filter(function (element, index) { 
        //   if(element.user.name != null && element.user.bio != null){
        //     if(element.user.bio.toLowerCase().includes(query) || element.user.name.toLowerCase().includes(query)){
        //       return element
        //     }
        //   }else if(element.user.bio != null){
        //     if(result.user.bio.toLowerCase().includes(query)){
        //       return element
        //     }
        //   }
        // });
    }
  }
})
</script>

<template>

<section class="py-1 text-center container main-search">
<div class="row py-lg-5">
  <div class="col-lg-6 col-md-8 mx-auto">
    <div class="input-group">
      <input id="searchInput" type="text" class="form-control" placeholder="Поиск">
      <button type="button" class="btn btn-search" @click="searchPost()">
        <i class="bi bi-search"></i>
      </button>
    </div>
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
