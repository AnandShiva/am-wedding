<template>
  <div id="container" class="container" @click="closeVideo">
      <img src="https://i.imgur.com/dGOOfnA.png" alt="image-top-right" class="static-img top-right-decoration">
      <img src="https://i.imgur.com/t6ffnbn.png" alt="image-top-left" class="static-img top-left-decoration"> 
    <div>
      <div class="invite-container">
        <span class='generic-text' v-show="guestUser"> {{prefix}} <span class="imp-ppl-text">{{guestUser}},</span></span>
        <span class='generic-text'>We,</span>
         <span class="bride-groom-text" >Mohana</span>
         <span class='generic-text valentine-red'>&</span>
        <span class="bride-groom-text">Anand</span>
        <span class='generic-text'>Invite you to our</span>
        <span class="event-text"> Wedding : </span>
        <span class='generic-text'>  13th June 7.30 AM to 9.30 AM</span>
        <span class='generic-text valentine-red'>&</span>
        <span class="event-text">Reception : </span>
        <span class='generic-text'> 12th June 6.30 PM to 9.30 PM</span>
        <div class='links'>
          <button @click="linkClicked(link,$event)" class='link-item big-button' :key="index" v-for="(link, index) in links" target="_blank" :href="link.linkAddress" > {{link.text}}</button>
        </div>
        <span class='generic-text'>It would mean the world to Us if you could wish Us on our special day!</span>
      </div>
      <figure>
          <audio
              autoplay
              src="https://freemusicspot.com/m/happy-together-turtles.mp3">
                  Your browser does not support the
                  <code>audio</code> element.
          </audio>
      </figure>
      <!-- <iframe class="video-container" v-if="showVideo" width="560" height="315" src="https://www.youtube.com/embed/1T5xkas3_h8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> -->

    </div>
  </div>
</template>

<script>
export default {
  created(){
    const plainUserName = this.getParams('un');
    const encodedUserName = this.getParams('us');
    const prefix = this.getParams('up');
    if(plainUserName){
      this.guestUser = plainUserName;
    }
    if(encodedUserName){
      this.guestUser = this.a2b(encodedUserName);
    }
    if(prefix){
      this.prefix = prefix
    }

  },
  mounted(){
    if(process.client){
      TweenLite.set(".leaf",{xPercent:"-50%",yPercent:"-50%"})
      var total = 30;
      var warp = document.getElementById("container"),	w = window.innerWidth , h = window.innerHeight;
      for (let i=0; i<total; i++){ 
          var Div = document.createElement('div');
          TweenLite.set(Div,{attr:{class:'leaf'},x:this.getRangeValues(-w/2,w/2),y:this.getRangeValues(-h/2,(-h/2)+1),z:this.getRangeValues(-200,200)});
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
      prefix: 'Hey',
      links: [
        {
          text: 'Watch Reception',
          linkAddress:'https://www.youtube.com/watch?v=5Qro_QYau0Y/'
        }, 

        {
          text: 'Watch Wedding',
          linkAddress:'https://www.youtube.com/watch?v=bz0JUKpaCnw/'
        },
                        {
          text: 'Venue',
          linkAddress: 'https://goo.gl/maps/yo6oTMLirQXmPP3Y6'
        },
      ]
    }
  }
}
</script>

<style>
@import url("https://fonts.googleapis.com/css?family=Open+Sans:400,400i,700");


body{
  overflow: hidden;
}
.imp-ppl-text{
  font-size: 32px;
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
.event-text{
  text-decoration: underline;
}

.links {
  padding-top: 16px;
  padding-bottom: 16px;
  display: flex;
  justify-content: center;

  flex-wrap: wrap;
}
.link-item{
  /* padding: 12px; */
  margin: 4px;
  margin-top: 8px;
  margin-bottom: 8px;
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
.valentine-red{
  color: #DC362D;
  font-weight: 700;
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

/* button css */
:root {
  --backgroundColor: rgba(246, 241, 209);
  --colorShadeA: rgb(106, 163, 137);
  --colorShadeB: rgb(121, 186, 156);
  --colorShadeC: rgb(150, 232, 195);
  --colorShadeD: rgb(187, 232, 211);
  --colorShadeE: rgb(205, 255, 232);
}

* {
  box-sizing: border-box;
}
*::before, *::after {
  box-sizing: border-box;
}
body {
  margin: 0;
  min-height: 100vh;
  background: var(--backgroundColor);
}
button {
  position: relative;
  display: inline-block;
  cursor: pointer;
  outline: none;
  border: 0;
  vertical-align: middle;
  text-decoration: none;
  font-size: 24px;
    color:var(--colorShadeA);
  font-weight: 700;
  font-family: inherit;
}

button.big-button {
  border: 2px solid var(--colorShadeA);
  border-radius: 1em;
  background: var(--colorShadeE);
  transform-style: preserve-3d;
  transition: all 175ms cubic-bezier(0, 0, 1, 1);
  padding: 8px;
  font-size:20px;
}
button.big-button::before {
  position: absolute;
  content: '';
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: var(--colorShadeC);
  border-radius: inherit;
  box-shadow: 0 0 0 2px var(--colorShadeB);
  transform: translate3d(0, 0.4em, -0.5em);
  transition: all 175ms cubic-bezier(0, 0, 1, 1);
}


button.big-button:hover {
  background: var(--colorShadeD);
  transform: translate(0, 0.375em);
}

button.big-button:hover::before {
  transform: translate3d(0, 0.75em, -1em);
}

/* button.big-button:active {
  transform: translate(0em, 0.75em);
}

button.big-button:active::before {
  transform: translate3d(0, 0, -1em);
  box-shadow: 0 0 0 2px var(--colorShadeB), 0 0.25em 0 0 var(--colorShadeB);
} */


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
