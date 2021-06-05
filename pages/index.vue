<template>
  <div class="container" @click="closeVideo">
    <div>
      <!-- <Logo /> -->
      <div class="invite-container">
        <span> Hey <span class="imp-ppl-text">There,</span></span>
        <span>We,</span>
         <span class="bride-groom-text" >Mohana</span>
         <span>&</span>
        <span class="bride-groom-text">Anand</span>
        <span> are getting married</span>
        <span>on 13th June</span>
        <span>@ SRI VATCHALA MARRIAGE HALL</span>
        <div class='links'>
          <span @click="linkClicked(link,$event)" class='link-item' :key="index" v-for="(link, index) in links" target="_blank" :href="link.linkAddress" > {{link.text}}</span>
        </div>
      </div>
      <iframe class="video-container" v-if="showVideo" width="560" height="315" src="https://www.youtube.com/embed/1T5xkas3_h8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

    </div>
  </div>
</template>

<script>
export default {
  methods:{
    linkClicked(link,event){
      if(link.linkAddress){
        window.open(link.linkAddress,'_blank');
        this.showVideo = false;
      }else{
        this.showVideo = true;
        event.stopPropagation();
      }
    },
    closeVideo(){
      if(this.showVideo){
        this.showVideo = false;
      }
    }
  },
  data(){
    return {
      showVideo: false,
      links: [
        {
          text: 'Venue',
          linkAddress: 'https://goo.gl/maps/yo6oTMLirQXmPP3Y6'
        },
        {
          text: 'Invitation',
          linkAddress: 'https://goo.gl/maps/yo6oTMLirQXmPP3Y6'
        },
        {
          text: 'Live Video',
          functionTrigger: ()=>{
                  this.showVideo = true
          },
        }
      ]
    }
  }
}
</script>

<style>

.imp-ppl-text{
  font-size: 40px;
  font-family: 'Great Vibes';
  font-style: normal;
  font-weight: 400;
}
.bride-groom-text{
  font-size: 48px;
  font-family: 'Great Vibes';
  font-style: normal;
  font-weight: 400;
}
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}
.invite-container{
  display: flex;
  flex-direction: column;
  font-size: 32px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
  display: flex;
  justify-content: center;
}
.link-item{
  padding: 12px;
  margin: 4px;
  background-color: lightblue ;
  border-radius: 20px;
}
a{
  text-decoration: none;
}
.video-container{
  position: fixed;
  right: 0;
  left: 0;
  top: 0;
  bottom: 0;
  margin: auto;
  z-index: 100;
}
</style>
