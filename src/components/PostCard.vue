<script lang="ts">
import axios from 'axios'
import cfgObj from '../cfg/config.js'

export default({
  data() {
    return {
      FullInfo: [],
      userPhoto: [],
      userInfo: [],
      postInfo: [],
      imageLoaded: false,
      imageDownloadUrl: "",
    }
  },
  async beforeMount () {
    try{
      var arrFilter = JSON.parse(localStorage.getItem('favCards')).filter(result => result.id == location.search.split('id=')[1]);
    }catch(err){
      var arrFilter = [];
    }

    if(localStorage.getItem('favCards') != null && arrFilter.length > 0){

        this.FullInfo = arrFilter[0]
        this.userPhoto = arrFilter[0].user.profile_image
        this.userInfo = arrFilter[0].user
        this.postInfo = arrFilter[0].urls
        this.imageLoaded = true;
    }else{
      await axios
      .get(cfgObj.url+'/photos/'+location.search.split('id=')[1]+'?client_id='+cfgObj.idClient)
      .then((response) => {
        this.FullInfo = response.data
        this.userPhoto = response.data.user.profile_image
        this.userInfo = response.data.user
        this.postInfo = response.data.urls
        //alert('DATA')
        this.imageLoaded = true;
        console.log(response.data);
      })
    }
  },
  mounted () {
    const id = location.search.split('id=')[1];
    const ixid = "Mnw0MDgxMDd8MHwxfGFsbHx8fHx8fHx8fDE2NzU5NzgzNTM";

    const endpoint = cfgObj.url + '/photos/' + id + '/download?ixid=' + ixid + '&client_id=' + cfgObj.idClient;

    axios
      .get(endpoint)
      .then((response) => {
        let url = response.data.url;
        this.toDataURL(url);
      })
      .catch((error) => {
        console.log(error);
      });
  },
  methods: {
    likeImg(data){
      try{
        var arrFilter = JSON.parse(localStorage.getItem('favCards')).filter(result => result.id == data.id);
      }catch(err){
        var arrFilter = [];
      }

      if(arrFilter.length == 0){
        data.activeClass = "active";
        if(localStorage.getItem('favCards') == null){
          var a = [];
          a.push(data);
          localStorage.setItem('favCards', JSON.stringify(a));
        }else{
          var a = JSON.parse(localStorage.getItem('favCards'));
          a.push(data);
          localStorage.setItem('favCards', JSON.stringify(a));
        }
      }else{
        var a = JSON.parse(localStorage.getItem('favCards')).filter(result => result.id != data.id);
        localStorage.setItem('favCards', JSON.stringify(a));
      }
      console.log("arrFilter")
      console.log(arrFilter)
    },
    async toDataURL(url) {
        const response = await fetch(url);
        const blob = await response.blob();
        const imageUrl = window.URL.createObjectURL(blob);
        // console.log(imageUrl);
        this.imageDownloadUrl = imageUrl;
    }
  }
})
</script>

<template>

<div class="col-md-8 mx-auto profile-card">
  <div>
    <div class="px-4 pt-0 pb-4 row">
      <div class="media align-items-end profile-head col">
        <div class="profile mr-3">
          <img v-if="imageLoaded" :src="userPhoto.large" alt="..." width="130" class="rounded mb-2 img-thumbnail">
          <p v-if="!imageLoaded" style="color: white">Loading image...</p>
        </div> 
        <div class="media-body mb-5 text-white">
          <h4 v-if="userInfo.name != null" class="mt-0 mb-0">{{userInfo.name}}</h4> 
          <h4 v-if="userInfo.name == null" class="mt-0 mb-0">Информация отсутствует</h4>
          <p v-if="userInfo.location != null" class="small mb-4"> 
            <i class="fas fa-map-marker-alt mr-2"></i>{{userInfo.location}}
          </p> 
          <p v-if="userInfo.location == null" class="small mb-4">Информация отсутствует</p> 
        </div>
        </div>
        <div class="media align-items-end profile-head col">
        <div class="p-4 d-flex justify-content-end text-center">
          <button type="button" :class="'btn btn-default fav-btn ' + FullInfo.activeClass" data-bs-toggle="button" aria-pressed="true" @click="likeImg(FullInfo)">
            <i class="bi bi-heart"></i>
          </button>
          <a :href="imageDownloadUrl" class="btn btn-warning download-btn" id="download" download>
            <i class="bi bi-download"></i>Скачать
          </a>
        </div>
      </div>
      </div>
    </div>
    <div class="py-4 px-4">  
      <div> 
        <div class="col-lg-12 mb-2 pr-lg-1 card-img">
          <img v-if="imageLoaded" :src="postInfo.full" alt="" class="img-fluid rounded shadow-sm" style="border-radius: 8px;">
          <p v-if="!imageLoaded">Loading image...</p>
        </div>    
      </div> 
    </div> 
  </div>
</template>

<style scoped>
</style>
