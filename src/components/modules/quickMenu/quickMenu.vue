<template>
  <div>
    <div class="wrap" ref="wrap" @click="cloceWrap"></div>
    <div class="quick-menu" ref="quickMenu" :style="quickMenuStyle">
      <div v-for="(n,key) in menuCount" class="sub-menu" :key="key" :style="getSubMenu(n-1)">
        <p :style="subMenuStyle" @mouseover.stop="mouseEnterSubMenu" @mouseout.stop="mouseOutSubMenu" @click="processCallback(menuUrlList[n-1].status)">
          <i  ref="icon" v-html="listText[n-1]"></i>
        </p>
      </div>
      <div class='menu' :style="menuStyle">
        <div class='core-menu' @click="toggleMenu">
          <!-- <div class='bar'></div> -->
          <Icon type="navicon"></Icon>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
	export default{
name:'quickMenu',
  props:{
    menuCount:{
      type: Number,
      required: true,
      default:5
    },
    listText:{
      type:Array,
      required: true
    },
    menuUrlList:{
      type:Array,
      required: true
    },
    backgroundColor:{
      type:String,
      default:'#20babb'
    },
    color:{
      type:String,
      default:'#fff'
    },
    isOpenNewTab:{
      type:Boolean,
      default:false
    },
    position:{
      type:String,
      default:'top-left'
    },
    bottom:{
      type:String,
      default:'100px'
    }
  },
  computed:{
    openNewTab(){
      return this.isOpenNewTab?'_blank':'_self'
    },
    quickMenuStyle(){
      const topPosition = {top:'100px'}, 
      bottomPosition={bottom:this.bottom},
      leftPosition = {left:'20px'},
      rightPosition = {right:'20px'}
      let style = this.isTop?topPosition:bottomPosition
      Object.assign(style, this.isLeft?leftPosition:rightPosition)
     // Object.assign(style,{transform: this.isLeft?"rotate(-180deg)":"rotate(180deg)"})
      return style
    },
    menuStyle(){
      return {
        backgroundColor: this.backgroundColor,
        color: this.color
      }
    },
    subMenuStyle(){
      const style = {
        backgroundColor: this.backgroundColor,
        color: this.color
      }
      return style
    },
   
    isTop(){
      return !!~this.position.toLowerCase().indexOf('top')
    },
    isLeft(){
      return !!~this.position.toLowerCase().indexOf('left')
    }
  },
  data(){
    return{
      menuSize:60,
      subMenu5:[[["90","7"],["64","-59.6"],["2.4","-93"],["-61","-66"],["-99","4"]],[["90","7"],["64","-59.6"],["2.4","-93"],["-61","-66"],["-99","4"]],[["90","-7"],["64","59.6"],["2.4","93"],["-61","66"],["-99","-4"]],[["90","-7"],["64","59.6"],["2.4","93"],["-61","66"],["-99","-4"]]],
      subMenu4:[[["87","7"],["41","-59.6"],["-36.6","-61"],["-84","4"]],[["87","7"],["41","-59.6"],["-36.6","-61"],["-84","4"]],[["-87","-7"],["-41","59.6"],["36.6","61"],["84","-4"]],[["-87","-7"],["-41","59.6"],["36.6","61"],["84","-4"]]],
      subMenu3:[[["-60","-38"],["0","-85"],["60","-60"]],[["-70","-10"],["0","-60"],["60","-10"]],[["-70","10"],["0","60"],["60","10"]],[["-60","22"],["-1","78"],["60","37"]]],
      subMenu2:[[["-60","0"],["0","-60"]],[["0","-60"],["72","0"]],[["0","72"],["72","0"]],[["-60","0"],["0","72"]]]
    }
  },
  methods:{
    getSubMenu(n){
      let menuPosition = this.menuCount===5?this.subMenu5:this.menuCount===4?this.subMenu4:this.menuCount===3?this.subMenu3:this.subMenu2
      menuPosition = this.isTop&&this.isLeft?menuPosition[2]:this.isTop&&!this.isLeft?menuPosition[1]:!this.isTop&&this.isLeft?menuPosition[3]:menuPosition[0]
      return {top:menuPosition[n][0]+'px',left:menuPosition[n][1]+'px'}
    },
    toggleMenu(e){
      let menuEl = this.$refs.quickMenu
      let menuIconEl = this.$refs.icon
      let wrap = this.$refs.wrap
      if(!~menuEl.className.indexOf(' active')){
         menuEl.className += ' active';
        menuIconEl.forEach( function(element, index) {
          element.className += ' menu-animate';
        });
        wrap.className += ' active'
      } else {
        menuEl.className = menuEl.className.replace(' active','')
        menuIconEl.forEach( function(element, index) {
        element.className = element.className.replace(' menu-animate','')
        });
        wrap.className= wrap.className.replace(' active','')
      }
      
    },
    processCallback(key){
      console.log(key)
      this.$emit('process',key)
      let menuEl = this.$refs.quickMenu
      let menuIconEl = this.$refs.icon
      let wrap = this.$refs.wrap
      menuEl.className = menuEl.className.replace(' active','')
      menuIconEl.forEach( function(element, index) {
      element.className = element.className.replace(' menu-animate','')
      });
      wrap.className= wrap.className.replace(' active','')
    },
    mouseEnterSubMenu(e){
      if(e.target.tagName==='A'){
        e.target.style.backgroundColor = this.lightenColor(this.backgroundColor, 20)
        // e.target.firstElementChild.style.backgroundColor = this.lightenColor(this.backgroundColor, 20)
      } else if(e.target.tagName==='I'){
        e.target.parentElement.style.backgroundColor = this.lightenColor(this.backgroundColor, 20)
        // e.target.style.backgroundColor = this.lightenColor(this.backgroundColor, 20)
      }
      
    },
    mouseOutSubMenu(e){
      if(e.target.tagName==='A'){
        e.target.style.backgroundColor = this.backgroundColor
        // e.target.firstElementChild.style.backgroundColor = this.backgroundColor
      }else if(e.target.tagName==='I'){
        e.target.parentElement.style.backgroundColor = this.backgroundColor
        // e.target.style.backgroundColor = this.backgroundColor
      }
      
    },
    lightenColor (hex, amt) {
      var usePound = false
      if (hex[0] === '#') {
        hex = hex.slice(1)
        usePound = true
      }
      var num = parseInt(hex, 16)
      var r = (num >> 16) + amt
      if (r > 255) r = 255
      else if (r < 0) r = 0
          var b = ((num >> 8) & 0x00FF) + amt
      if (b > 255) b = 255
      else if (b < 0) b = 0
      var g = (num & 0x0000FF) + amt
      if (g > 255) g = 255
      else if (g < 0) g = 0
      return (usePound ? '#' : '') + (g | (b << 8) | (r << 16)).toString(16)
    },
    cloceWrap () {
        let menuEl = this.$refs.quickMenu
        let menuIconEl = this.$refs.icon
        let wrap = this.$refs.wrap
        menuEl.className = menuEl.className.replace(' active','')
        menuIconEl.forEach( function(element, index) {
        element.className = element.className.replace(' menu-animate','')
        });
        wrap.className= wrap.className.replace(' active','')
    }
  }
}
</script>
<style lang="less">
.menu-animate {
  -webkit-animation: bounce 1s linear 1s;
  -moz-animation: bounce 1s linear 1s;
  animation: bounce 1s linear 1s;
}
.wrap{
  position: fixed;
  width:100%;
  height:100%;
  background: rgba(0, 0, 0, 0.4);
  top:0;
  left:0;
  -webkit-transition: all 0.5s ease;
  -moz-transition: all 0.5s ease;
  transition: all 0.5s ease;
  opacity: 0;
  display: none;
  z-index: 22;
}
.wrap.active{
  opacity: 1;
  display: block;
}
.quick-menu {
  color: #fff;
  position: fixed;
  width: 40px;
  height: 40px;
  -webkit-transition: all 1s ease;
  -moz-transition: all 1s ease;
  transition: all 1s ease;
  right: 30px;
  z-index: 23;
  > .menu {
    display: block;
    position: absolute;
    border-radius: 50% !important;
    width: 40px;
    height: 40px;
    text-align: center;
    box-shadow: 0 3px 10px rgba(0, 0, 0, 0.23), 0 3px 10px rgba(0, 0, 0, 0.16);
    color: #fff;
    -webkit-transition: all 1s ease;
    -moz-transition: all 1s ease;
    transition: all 1s ease;
    .core-menu {
      width: 100%;
      height: 100%;
      position: absolute;
      left: 0px;
      top: 0px;
      width: 40px;
      height: 40px;
      -webkit-transform: rotate(180deg);
      -moz-transform: rotate(180deg);
      -ms-transform: rotate(180deg);
      -o-transform: rotate(180deg);
      transform: rotate(180deg);
      -webkit-transition: all 1s ease;
      -moz-transition: all 1s ease;
      transition: all 1s ease;
      font-size: 2rem;
      line-height: 40px;
      // .bar {
      //   -webkit-transition: all 1s ease;
      //   -moz-transition: all 1s ease;
      //   transition: all 1s ease;
      //   width: 28px;
      //   height: 3px;
      //   background: #fff;
      //   position: absolute;
      //   top: 30%;
      //   margin-top: -1.5px;
      //   left: 6px;
      //   -webkit-transform-origin: 0% 50%;
      //   -moz-transform-origin: 0% 50%;
      //   -ms-transform-origin: 0% 50%;
      //   -o-transform-origin: 0% 50%;
      //   transform-origin: 0% 50%;
      //   &:before ,&:after{
      //     -webkit-transition: all 1s ease;
      //     -moz-transition: all 1s ease;
      //     transition: all 1s ease;
      //     content: '';
      //     width: 28px;
      //     height: 3px;
      //     background: #fff;
      //     position: absolute;
      //     left: 0px;
      //     -webkit-transform-origin: 0% 50%;
      //     -moz-transform-origin: 0% 50%;
      //     -ms-transform-origin: 0% 50%;
      //     -o-transform-origin: 0% 50%;
      //     transform-origin: 0% 50%;
      //   }
      //   &:before{
      //     margin-top: 30%;
      //   }
      //   &:after {
      //     margin-top: 60%;
      //   }
      // }
    }
  }
  .sub-menu{
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    box-sizing: border-box;
    position: absolute;
    width: 3rem;
    height: 3rem;
    font-size: 0.8rem;
    text-align: center;
    line-height:3rem;
    border-radius: 50% !important;
    -webkit-transform: scale(0);
    -moz-transform: scale(0);
    -ms-transform: scale(0);
    -o-transform: scale(0);
    transform: scale(0);
    -webkit-transition: all 1s ease;
    -moz-transition: all 1s ease;
    transition: all 1s ease;
    p{
      outline: none;
      text-decoration: none;
      display: inline-block;
      border-radius: 50% !important;
      width: 100%;
      height: 100%;
      i {
        outline: none;
        font-style: normal;
      font-size: 0.3rem;
        background:transparent;
        &:before{
          vertical-align: middle;
        }
      }
      &:hover {
        cursor: pointer;
      }
    }
  }
  &.active{
    -webkit-transform: rotate(0deg)!important;
      -moz-transform: rotate(0deg)!important;
      -ms-transform: rotate(0deg)!important;
      -o-transform: rotate(0deg)!important;
      transform: rotate(0deg)!important;
      .menu{
        // .bar {
        //   top: 50%;
        //   margin-top: -1.5px;
        //   left: 50%;
        //   margin-left: -12px;
        //   -webkit-transform-origin: 50% 50%;
        //   -moz-transform-origin: 50% 50%;
        //   -ms-transform-origin: 50% 50%;
        //   -o-transform-origin: 50% 50%;
        //   transform-origin: 50% 50%;
        //   -webkit-transform: rotate(405deg);
        //   -moz-transform: rotate(405deg);
        //   -ms-transform: rotate(405deg);
        //   -o-transform: rotate(405deg);
        //   transform: rotate(405deg);
        //       &:before {
        //         -webkit-transform-origin: 50% 50%;
        //         -moz-transform-origin: 50% 50%;
        //         -ms-transform-origin: 50% 50%;
        //         -o-transform-origin: 50% 50%;
        //         transform-origin: 50% 50%;
        //         -webkit-transform: rotate(-450deg);
        //         -moz-transform: rotate(-450deg);
        //         -ms-transform: rotate(-450deg);
        //         -o-transform: rotate(-450deg);
        //         transform: rotate(-450deg);
        //         margin-top: 0px;
        //       }
        //   &:after{
        //     opacity: 0;
        //   }
        // }
      }
      .sub-menu{
        -webkit-transform: scale(1);
        -moz-transform: scale(1);
        -ms-transform: scale(1);
        -o-transform: scale(1);
        transform: scale(1); 
        -webkit-transition: all 1s ease;
        -moz-transition: all 1s ease;
        transition: all 1s ease;
      }
    }
}
@-webkit-keyframes bounce {
  0%,100% {
    -webkit-transform: translateY(0px);
  }
  10% {
    -webkit-transform: translateY(6px);
  }
  30% {
    -webkit-transform: translateY(-4px);
  }
  70% {
    -webkit-transform: translateY(3px);
  }
  90% {
    -webkit-transform: translateY(-2px);
  }
}
@-moz-keyframes bounce {
  0%,100% {
    -moz-transform: translateY(0px);
  }
  10% {
    -moz-transform: translateY(6px);
  }
  30% {
    -moz-transform: translateY(-4px);
  }
  70% {
    -moz-transform: translateY(3px);
  }
  90% {
    -moz-transform: translateY(-2px);
  }
}
@keyframes bounce {
  0%,100% {
    -webkit-transform: translateY(0px);
    -moz-transform: translateY(0px);
    -ms-transform: translateY(0px);
    -o-transform: translateY(0px);
    transform: translateY(0px);
  }
  10% {
    -webkit-transform: translateY(6px);
    -moz-transform: translateY(6px);
    -ms-transform: translateY(6px);
    -o-transform: translateY(6px);
    transform: translateY(6px);
  }
  30% {
    -webkit-transform: translateY(-4px);
    -moz-transform: translateY(-4px);
    -ms-transform: translateY(-4px);
    -o-transform: translateY(-4px);
    transform: translateY(-4px);
  }
  70% {
    -webkit-transform: translateY(3px);
    -moz-transform: translateY(3px);
    -ms-transform: translateY(3px);
    -o-transform: translateY(3px);
    transform: translateY(3px);
  }
  90% {
    -webkit-transform: translateY(-2px);
    -moz-transform: translateY(-2px);
    -ms-transform: translateY(-2px);
    -o-transform: translateY(-2px);
    transform: translateY(-2px);
  }
}
</style>