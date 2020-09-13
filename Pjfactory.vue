<template>
  <div class="Pjfactory" >

    <div class="container-fluid">






        <div style="position: absolute;" v-for="node in tree">
          <transition v-on:enter="enter" v-on:leave="leave">
          <img :name="node.idx" class="img-fluid" v-if="node.show" :src="require('@/assets/pjfactory/test_image_'+node.idx+'.jpeg')"  alt="Responsive image">
          </transition>

            <transition-group v-on:enter="btnEnter" v-on:leave="btnLeave" >
            <button v-if="!!node.child" v-for="childIdx in node.child" v-bind:key="childIdx" v-show="node.show" class="btn"
              v-on:click="node.show=!node.show; tree[childIdx-1].show=!tree[childIdx-1].show; position=tree[childIdx-1].position; stateIdx=childIdx; back=false;"
              :style="'position: absolute; left: calc( '+tree[childIdx-1].position[0]+'% - 20px ); top:calc( '+tree[childIdx-1].position[1]+'% - 20px );'">
                <img :src="require('@/assets/pjfactory/icon.png')" v-if="!!node.child">
            </button>
            </transition-group>

          <transition v-on:enter="btnEnter" v-on:leave="btnLeave" >
          <button v-if="!!node.parent"  v-show="node.show"  class="btn "
           v-on:click="tree[node.parent-1].show=!tree[node.parent-1].show; node.show=!node.show; stateIdx=node.parent; position=node.position; back=true;"
           style="position: absolute; left: calc( 50% - 50px ); top:calc( 80% - 50px );" >
            <img :src="require('@/assets/pjfactory/back.png')">
          </button>
          </transition>
        </div>





      </div>

    </div>



    </template>

    <script>
    import Velocity from 'velocity-animate'

    export default {
    name: 'Pjfactory',
    props:['deviceSize','isAdmin','isLogined','profile'],
    data(){
    return{

       stateIdx:1,
       back:false,
       position:[0,0],
       tree:[
              { idx:1,
                show:true,
                position:null,
                child:[2,3],
                parent:null
              },
              { idx:2,
                show:false,
                position:[50,70],
                child:[4],
                parent:1
              },
              { idx:3,
                show:false,
                position:[5,40],
                child:null,
                parent:1
              },
              { idx:4,
                show:false,
                position:[50,30],
                child:null,
                parent:2
              }

            ]
    }
    },
    components: {
    },
    created(){

    },
    methods:{

     enter(el,done) {

       if(!this.back){
         if(Number(el.name)< this.stateIdx){
           this.parentEnter(el,done);
         }else{
           this.childEnter(el,done);
         }

       }else{
         if(Number(el.name) > this.stateIdx){
           this.childEnter(el,done);
         }else{
           this.parentEnter(el,done);
         }
       }


     },
     leave(el,done) {
       if(!this.back){
         if(Number(el.name)< this.stateIdx){
           this.parentLeave(el,done);
         }else{
           this.childLeave(el,done);
         }

       }else{
         if(Number(el.name) > this.stateIdx){
           this.childLeave(el,done);
         }else{
           this.parentLeave(el,done);
         }
       }


     },
     parentEnter(el,done){
       console.log("parentEnter");
       Velocity(el, { opacity: 1, translateX: (50-this.position[0])+'%', translateY: (50-this.position[1])+'%', scale: 2 }, { duration: 0 })
       Velocity(el, { opacity: 1, translateX: '0%', translateY: '0%', scale: 1 }, { duration: 300 },{ complete: done })
     },
     parentLeave(el,done){
       console.log("parentLeave");
       Velocity(el, { opacity: 1,translateX: (50-this.position[0])+'%', translateY: (50-this.position[1])+'%', scale:2}, { complete: done })
     },


     childEnter(el,done){
       console.log("childEnter");
       Velocity(el, { opacity: 1, translateX: (this.position[0]-50)+'%', translateY: (this.position[1]-50)+'%', scale: 0 }, { duration: 0 })
       Velocity(el, { opacity: 1, translateX: '0%', translateY: '0%', scale: 1 }, { duration: 300 },{ complete: done })
     },
     childLeave(el,done){
       console.log("childLeave");
       Velocity(el, { opacity: 1,translateX: (this.position[0]-50)+'%', translateY: (this.position[1]-50)+'%', scale:0}, { complete: done })
     },
     btnEnter(el,done){
       Velocity(el, { opacity: 0}, { duration: 0 })
       Velocity(el, { opacity: 1}, { duration: 300 },{ complete: done })
     },
     btnLeave(el,done){
       Velocity(el, { opacity: 0}, { duration: 300 },{ complete: done })
     },

    },

    }
    </script>

    <style lang="scss">
    .pjfactory{
      position: static;
      height:1080 !important;
      width:1920 !important;
      display: inline;
    }

    </style>
      
