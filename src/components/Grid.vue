<template>
    <div class="grid">
        <div class="vide">
            
        </div >

        <div class="headerparent">
           <monthandyear :cmonth="cmonth" :cyear="cyear"></monthandyear>
        <div class="header">
            <div class="days" v-for="(day,index) in days" :key="index" >
                {{weekdays[day.weekday]}} {{day.day}} 
            </div>
        </div>
        </div>

        <div class="ressources">
                <div class="ressourcesList">
                <div class="ressource" v-for="(res,index) in ressources" :key="index+155">
                    <div >
                       ressource {{res}}
                    </div>
                </div>
            </div>
        </div>
        <div class="bodyy">
            <!--div class="bodyDaysParent">
                <div class="dayBody" :name="day.completeDate" v-for="(day,index) in days" v-bind:key="index+1" :index="index">
                     
                </div>
            </div>
            class="bodyRowsparent"
            -->
            
                <div 
                :id="'row'+ind"
                class="roww" 
                 v-for="(res,ind) in scheduler" 
                :key="ind+1" :index="ind"
                 
                :name="res.title">
               <div class="bodyDaysParent">
                <div class="dayBody" :name="day.completeDate" v-for="(day,index) in days" v-bind:key="'row'+res.title+index+1" :index="index" :rowIndex="ind" :room="res.title" @click="getRows" @dragover="canIRes">
                     
                </div>
            </div>
                <event 
                v-for="(e,index) in res.events "
                :key="index" :test="'test'"
                :xleft="e.left"
                :ytop="e.top"
                :propheight="e.height"
                :propwidth="e.width">
                </event>
                </div>
            
        </div>
        
    </div>
    
</template>
<script>
import OneEvent from './OneEvent.vue'
/*var componentEvent=Vue.extend(OneEvent);
var instanceEvent=new componentEvent();
instanceEvent.$mount();
this.$refs.container.appendChild(instanceEvent.$el);*/
import {bus} from '../main'
import monthAndYear from './MonthAndYear.vue'



export default {
    
        data(){
            return {
                bodyDaysRects:[],
                ressources:["1","2","3","4","5","6","7","8","9","10"],
               weekdays:["Sun","Mon","Tue","Wed","Thu","Fri","Sat"],
               scheduler:
               [
                   {id:0,rowIndex:0,title:"ressource 1",
                        events:[{id:0,text:"reserved",start:"00/00/00",end:"00/00/00",width:40,height:15,top:8,left:58},
                                 {id:0,text:"reserved",start:"00/00/00",end:"00/00/00",width:40,height:15,top:8,left:269}
                                ]
                    },
               {id:1,rowIndex:1,title:"ressource 2",
                        events: [ {id:0,text:"reserved",start:"00/00/00",end:"00/00/00",width:80,height:15,top:8,left:80},
                                  {id:1,text:"reserved",start:"00/00/00",end:"00/00/00",width:40,height:15,top:8,left:269}
                                ]
                },
               {id:2,rowIndex:2,title:"ressource 3",
                        events:[{id:2,text:"reserved",start:"00/00/00",end:"00/00/00",width:40,height:15,top:8,left:58},
                                    {id:3,text:"reserved",start:"00/00/00",end:"00/00/00",width:60,height:15,top:8,left:269}
                                ]
                },
                {id:3,rowIndex:3,title:"ressource 4",
                        events:[{id:4,text:"reserved",start:"00/00/00",end:"00/00/00",width:40,height:15,top:8,left:78},
                                    {id:5,text:"reserved",start:"00/00/00",end:"00/00/00",width:40,height:15,top:8,left:269}
                                ]
                },
                {id:4,rowIndex:4,title:"ressource 5",
                        events:[{id:6,text:"reserved",start:"00/00/00",end:"00/00/00",width:40,height:15,top:8,left:78},
                                    {id:7,text:"reserved",start:"00/00/00",end:"00/00/00",width:40,height:15,top:8,left:269}
                                ]
                },
                 {id:5,rowIndex:5,title:"ressource 6",
                        events:[{id:8,text:"reserved",start:"00/00/00",end:"00/00/00",width:40,height:15,top:8,left:78},
                                    {id:9,text:"reserved",start:"00/00/00",end:"00/00/00",width:40,height:15,top:8,left:269}
                                ]
                },
                {id:6,rowIndex:6,title:"ressource 7",
                        events:[{id:10,text:"reserved",start:"00/00/00",end:"00/00/00",width:40,height:15,top:8,left:78},
                                    {id:11,text:"reserved",start:"00/00/00",end:"00/00/00",width:40,height:15,top:8,left:269}
                                ]
                },
                {id:7,rowIndex:7,title:"ressource 8",
                        events:[{id:12,text:"reserved",start:"00/00/00",end:"00/00/00",width:40,height:15,top:8,left:78},
                                    {id:13,text:"reserved",start:"00/00/00",end:"00/00/00",width:40,height:15,top:8,left:269}
                                ]
                },
                 {id:8,rowIndex:8,title:"ressource 9",
                        events:[{id:14,text:"reserved",start:"00/00/00",end:"00/00/00",width:40,height:15,top:8,left:78},
                                    {id:15,text:"reserved",start:"00/00/00",end:"00/00/00",width:40,height:15,top:8,left:269}
                                ]
                },
                 {id:9,rowIndex:9,title:"ressource 10",
                        events:[{id:16,text:"reserved",start:"00/00/00",end:"00/00/00",width:40,height:15,top:8,left:78},
                                    {id:17,text:"reserved",start:"00/00/00",end:"00/00/00",width:40,height:15,top:8,left:269}
                                ]
                },

                ]
            };
        },props:["days","cmonth","cyear","daysInMonth"],
        components:{
            'event':OneEvent,
            'monthandyear':monthAndYear,
            
        },updated(){
           bus.$emit('changeTheGrid',this.daysInMonth);
            /*
           this.scheduler.forEach(e => {
               e.events.forEach(v=>{
                   console.log("updated");
            console.log(v.left)
            console.log("x: "+v.left+"y: "+v.top)
            //console.log("length "+this.event.length)
            bus.$emit('changeStyle',{x:v.left,y:v.top}); 
               });
           
            
        });*/
        }
       
        ,mounted(){
            // this bus emit is for changing the css grif template of the grid
            bus.$emit('changeTheGrid',this.daysInMonth);
            //this is for returning the daybody clientRects
            console.log("err dehnik ar dayi");
            console.log(this.days)
             console.log("updzzzzzzaaaaateeeed");
       console.log(document.getElementById("row0").childNodes[0].childNodes);
      let bodyDays=document.getElementById("row0").childNodes[0].childNodes;
      bodyDays.forEach(bd=>{
        console.log(bd.getClientRects());
        this.bodyDaysRects.push({leftbd:bd.getClientRects()[0].left,rightbd:bd.getClientRects()[0].right})
      })
      console.log(this.bodyDaysRects)
          /* 
           this.scheduler.forEach(e => {
               e.events.forEach(v=>{
                   console.log("updated");
            console.log(v.left)
            console.log("x: "+v.left+"y: "+v.top)
            //console.log("length "+this.event.length)
            bus.$emit('changeStyle',{x:v.left,y:v.top}); 
               })
               
            })
           */
           
    
        },
        computed:{
            
            cssVars(){
               return { '--grid-template-columns': `repeat(${this.n},1fr)`}
               
                }
               },
               created(){
                   bus.$on('changeTheGrid',(n)=>{
                       this.change(n);
                   })
                   bus.$on('eventClicked',(data)=>{
                       this.getIntervalDate(data.leftEv,data.rightEv);
                   })
                   
               },
         methods: {
            change(num){
                console.log("faut voir aywaq wayi sexecuuuuute")
                this.h=this.d;
                document.querySelector(".header").style.setProperty('--grid-template-columns','repeat('+num+',3fr)');
                
                document.querySelectorAll(".bodyDaysParent").forEach(e=>{
                    e.style.setProperty('--grid-template-columns','repeat('+num+',3fr)');
                })
            },
            getRows(e){
               // console.log(e.target.attributes.name.value+" at row "+e.target.attributes.index.value);
                //if(e.target.className=='row'){
               // console.log("aqline dayi"+e.target.attributes.index)
                //console.log(e.target.attributes.index.value);
                console.log(e);
                if(!this.existingEvent(e)){
                console.log("e.target ayiniiiii")
                console.log(parseFloat(e.target.offsetWidth)-parseFloat(e.target.clientWidth));
                this.scheduler[parseInt(e.target.attributes.rowIndex.value)].events.push({width:40,height:20,left:parseFloat(window.getComputedStyle(e.target).width)*(parseInt(e.target.attributes.index.value)),top:8});
                console.log(this.scheduler);
                console.log(parseFloat(window.getComputedStyle(e.target).width)+" X "+parseInt(e.target.attributes.index.value));
                
                console.log("clientX");
                console.log(e.clientX)
                }else{
                    console.log("existing event can't reserve")
                }
               // let roww=document.querySelector("#row0");
               //let res= roww.querySelectorAll(".resizable");
               //console.log(res)
               // this.getIntervalDate(e.clientX,e.clientX)
               // }
               console.log("existingEvent");
              console.log(this.existingEvent(e));
           },
           getIntervalDate(leftEv,rightEv){
               let bodyyElement=document.querySelector(".bodyy");
               function bodyDayIndexOfLeftEv(dom){
                   return dom.leftbd<leftEv+bodyyElement.scrollLeft && dom.rightbd>leftEv+bodyyElement.scrollLeft;
               }
               function theBodyDayIndexOfTheRightEv(dom){
                   return dom.leftbd<rightEv+bodyyElement.scrollLeft && dom.rightbd>rightEv+bodyyElement.scrollLeft;
               }
               let indexOfTheLeftEv= this.bodyDaysRects.findIndex(bodyDayIndexOfLeftEv);
               let indexOfTheRightEv=this.bodyDaysRects.findIndex(theBodyDayIndexOfTheRightEv);
                
           console.log("theBodyDayIndexOfTheRightEv");
           bus.$emit("reservationModified",{start:this.days[indexOfTheLeftEv].completeDate,end:this.days[indexOfTheRightEv].completeDate})
           console.log(this.days[indexOfTheRightEv].completeDate);
           console.log(this.days[indexOfTheLeftEv].completeDate)
           },
           existingEvent(e){
               console.log("events");
                let events= e.target.parentNode.parentNode.querySelectorAll(".resizable");
            let v=[];
               events.forEach(ev=>{
                  //console.log(ev.getClientRects())
                    v.push({left:ev.getClientRects()[0].left,right:ev.getClientRects()[0].right});
                })
               console.log(events);
               console.log(v);
              return v.some(item=>{return e.clientX<=item.right && e.clientX>=item.left});

           },
           canIRes(e){
               console.log(e.clientX);
           }
        
            
            
        }
}
        

</script>

<style src="./Liste.css">

</style>