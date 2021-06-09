<template>
  <div id="app">
   <!--<header-top></header-top>
    <contenu></contenu>-->
    <button v-on:click="prev">Prev</button>
     <button v-on:click="next">Next</button>
    
     
   
  <parentgrid></parentgrid>
    <modal v-bind:revele="revele"  v-bind:toggleModale="toggleModale" :chambre="chambre" :arrive="arrive" :depart="depart"></modal>
  </div>
</template>

<script>
//import ResizeVue from './components/Resize.vue';
import {bus} from './main';
import Modal from './components/Modal.vue'
//import Contenu from './components/Contenu.vue'
import ParentGrid from './components/ParentGrid.vue'
// import Header from './components/Header.vue'
export default {
  name: 'App',
  data() {
    return {
     revele:false,
     
     chambre:"chambre 101",
     arrive:"01/01/2021",
     depart:"30/01/2021"
    }
  },
  components: {
    //'contenu':Contenu,
    //'header-top':Header
    'parentgrid':ParentGrid,
    'modal':Modal
   //'resize':ResizeVue
    
  },updated(){
   
  },created(){
    bus.$on("reservationModified",d=>{this.setReservation(d)});
  },
   methods:{
    next:function(){
    bus.$emit('nextFired');
  },
  prev:function(){
    bus.$emit('prevFired')
  },
  toggleModale:function(){
    this.revele=!this.revele;
  },
  setReservation:function(data){
    this.revele=true;
    this.arrive=data.start.getDate()+"/"+data.start.getMonth()+"/"+data.start.getFullYear();
    this.depart=data.end.getDate()+"/"+data.end.getMonth()+"/"+data.end.getFullYear();
  }
  },
  
}
</script>

<style>

</style>
