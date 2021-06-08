<template>
  <div id="container" class="container" @click="closeVideo">
      <img src="https://i.imgur.com/dGOOfnA.png" alt="image-top-right" class="static-img top-right-decoration">
      <img src="https://i.imgur.com/t6ffnbn.png" alt="image-top-left" class="static-img top-left-decoration"> 
    <div>
      <div class="invite-container">
        <span class='generic-text' v-show="guestUser"> Hey <span class="imp-ppl-text">{{guestUser}},</span></span>
        <span class='generic-text'>We,</span>
         <span class="bride-groom-text" >Mohana</span>
         <span class='generic-text'>&</span>
        <span class="bride-groom-text">Anand</span>
        <span class='generic-text'>Invite you to our wedding</span>
        <span class='generic-text'>on 13th June</span>
        <span class='generic-text'>@ SRI VATCHALA MARRIAGE HALL</span>
        <div class='links'>
          <span @click="linkClicked(link,$event)" class='link-item' :key="index" v-for="(link, index) in links" target="_blank" :href="link.linkAddress" > {{link.text}}</span>
        </div>
        <span class='generic-text'>It would mean the world to Us if you could wish Us on our special day!</span>
      </div>
      <!-- <iframe class="video-container" v-if="showVideo" width="560" height="315" src="https://www.youtube.com/embed/1T5xkas3_h8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> -->

    </div>
  </div>
</template>

<script>
export default {
  created(){
    const plainUserName = this.getParams('un');
    const encodedUserName = this.getParams('us');
    if(plainUserName){
      this.guestUser = plainUserName;
    }
    if(encodedUserName){
      this.guestUser = this.a2b(encodedUserName);
    }

  },
  mounted(){
    if(process.client){
      TweenLite.set(".leaf",{xPercent:"-50%",yPercent:"-50%"})
      var total = 30;
      var warp = document.getElementById("container"),	w = window.innerWidth , h = window.innerHeight;
      for (let i=0; i<total; i++){ 
          var Div = document.createElement('div');
          TweenLite.set(Div,{attr:{class:'leaf'},x:this.getRangeValues(-w,w),y:this.getRangeValues(-h/2,(-h/2)+1),z:this.getRangeValues(-200,200)});
          warp.appendChild(Div);
          this.animate(Div,h);
        }
      }
  },
  methods:{
    getRangeValues(min, max){
      return min+Math.random()*(max-min);
    },
    animate(elm,h,){
        TweenMax.to(elm,this.getRangeValues(6,15),{y:h+100,ease:Linear.easeNone,repeat:-1,delay:-15});
        TweenMax.to(elm,this.getRangeValues(4,8),{x:'+=100',rotationZ:this.getRangeValues(0,180),repeat:-1,yoyo:true,ease:Sine.easeInOut});
        TweenMax.to(elm,this.getRangeValues(2,8),{rotationX:this.getRangeValues(0,360),rotationY:this.getRangeValues(0,360),repeat:-1,yoyo:true,ease:Sine.easeInOut,delay:-5});
    },
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
    },
    getParams  (key) {
      var params = this.$route.query || {};
      return params[key]
    },
    a2b(a) {
      // nuxt being SSRed, does not understand btoa functions. This is a pure JS alternative to it. 
      var b, c, d, e = {}, f = 0, g = 0, h = "", i = String.fromCharCode, j = a.length;
      for (b = 0; 64 > b; b++) e["ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/".charAt(b)] = b;
      for (c = 0; j > c; c++) for (b = e[a.charAt(c)], f = (f << 6) + b, g += 6; g >= 8; ) ((d = 255 & f >>> (g -= 8)) || j - 2 > c) && (h += i(d));
      return h;
    }
  },
  data(){
    return {
      showVideo: false,
      guestUser: '',
      links: [
        {
          text: 'Venue',
          linkAddress: 'https://goo.gl/maps/yo6oTMLirQXmPP3Y6'
        },
        {
          text: 'Live Video',
          linkAddress:'https://www.livewedding.org/anandmohana/',
          functionTrigger: ()=>{
                  this.showVideo = true
          },
        },
        {
          text: 'Invitation',
          linkAddress: 'https://drive.google.com/file/d/1LT1zZz3Rt3lRfnJsobR03_9hZiG83gCO/view?usp=sharing'
        }
      ]
    }
  }
}
</script>

<style>

body{
  overflow: hidden;
}
.imp-ppl-text{
  font-size: 40px;
  /* font-family: 'Great Vibes';
  font-style: normal;
  font-weight: 400; */
}
.bride-groom-text{
  font-size: 48px;
  font-family: 'Great Vibes';
  font-style: normal;
  font-weight: 600;
  color: #a02fa0;
}
.container {
  margin: 0 auto;
  min-height: 100vh;
  min-width: 100vw;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  box-sizing: border-box;

}
.invite-container{
  display: flex;
  flex-direction: column;
  font-size: 24px;
}

.generic-text{
  padding: 4px;
  font-family: 'Courgette';
  font-style: normal;
  font-weight: 300;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 16px;
  padding-bottom: 16px;
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
}
.link-item{
  padding: 12px;
  margin: 4px;
  background-color: lightblue ;
  border-radius: 20px;
  cursor: pointer;
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
.top-right-decoration{
  position: fixed;
  top:0;
  right:0;
}
.top-left-decoration{
  position: fixed;
  top:0;
  left:0;
}
.static-img {
  max-width: 100%;
  height: auto;
}
@media (max-width:900px) {
	.static-img {
		max-width: 40%;
	}
}


/* animation css */

.leaf{
  width:35px;
  height:35px;
  position:fixed;
  background: url(https://image.ibb.co/kyUHab/rose.png);
  background-size: 100% 100%;
}

html, body, #container {width:100%; height:100%; }

</style>
