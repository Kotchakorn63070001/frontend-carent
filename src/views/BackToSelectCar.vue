<template>
<div>
      <nav class="navbar is-info" role="navigation" aria-label="main navigation">
  <div class="navbar-brand">
    
    <a class="navbar-item" >
      <span class="icon-text has-text-warning">
        <span><h1 class="title is-4 has-text-warning">CARENT</h1></span>
        <span class="icon is-medium	">
          <font-awesome-icon icon="fa-solid fa-car-side" />
        </span>
      </span>
    </a>
  </div>
  </nav> 
    <div class="container-fluid ">
      
       
        <div class="card rounded-3 py-5 px-5 mt-5 " style="width: 60%; margin-left:20%;min-width:500px">
            <div class="content">
                <h2 >เลือกรถ</h2>
                
                <div class="container pb-3">
                    <div class="columns ">
                        <div class="column">
                            <h6>Type:</h6>
                        </div>
                        <!-- <div class="column">
                            <h6>Brand:</h6>
                        </div> -->
                    </div>
                    <div class="columns " style="margin-top:-5%">
                        <div class="column">
                            <div class="select">
                                <select class="select" name="type" v-model="order_carType"  @change="selectType(order_carType)">
                                    <option  v-for="type in getOnlyType" v-bind:key="type.type">{{type.type}}</option>
                                </select>
                            </div>
                        </div>
                        <!-- <div class="column">
                            <div class="select">
                                <select class="select" name="model" v-model="order_carBrand">
                                    <option  v-for="brand in getOnlyBrand" v-bind:key="brand">{{brand.brand}}</option>
                                </select>
                            </div>
                        </div> -->
                    </div>
                    

                </div>
                <div class="content">
                    <h6>Model:</h6>
                    <div class="columns is-multiline">
                        <!-- <div v-if="order_carType!==''"></div> -->
                        <div class="column is-4 " v-for="car in carByType" v-bind:key="car._id">

                            <div class="card is-hoverable" @click="selectModel(car._id,car.model)"  :style="{ border: car._id==order_carId ? '3px solid red' : '5px solid white'}"  >
                                <!-- <div :class="[car._id==order_carId ? has-background-danger : '', has-background-light]" > -->
                                 <figure class="image is-256x256 ">
                                    <img v-bind:src="`data:image/png;base64, ${car.image.data}`">
                                </figure>
                                    
                                <div class="px-5 py-5">
                                    <h5><span class="has-text-weight-medium">{{car.model}}</span><span style="margin-left:20px;" class="is-size-7 has-text-danger">{{car.brand}}</span></h5>
                                       
                                    <div class="small">
                                        <div>รองรับได้สูงสุด {{car.numOfSeat}} คน</div>
                                        <div>ราคา {{car.price}} บาทต่อวัน</div>
                                    </div>
                                </div>
                                </div>
                            <!-- </div> -->

                            </div>
                            <div v-if="carByType.length==0">
                                <div class="columns is-multiline">
                            <div class="column is-4 "  v-for="car in oldCar" v-bind:key="car._id">

                            <div class="card is-hoverable" @click="selectModel(car._id,car.model)" :style="{ border: car._id==order_carId ? '3px solid red' : '5px solid white'}"  >
                               
                                 <figure class="image is-256x256 ">
                                    <img v-bind:src="`data:image/png;base64, ${car.image.data}`">
                                </figure>
                                    
                                <div class="px-5 py-5">
                                    <h5><span class="has-text-weight-medium">{{car.model}}</span><span style="margin-left:20px;" class="is-size-7 has-text-danger">{{car.brand}}</span></h5>
                                       
                                    <div class="small">
                                        <div>รองรับได้สูงสุด {{car.numOfSeat}} คน</div>
                                        <div>ราคา {{car.price}} บาทต่อวัน</div>
                                    </div>
                                </div>
                                </div>
                          

                            </div></div></div>
                        </div>
                    </div>
                </div>
                <div class="content">
            <h3 class="pb-2 columns">เลือกวันที่-เวลา</h3>
            <label class="pl-6 " > วันรับรถ : </label><input type="date" v-model="order_dateStert"/>
            <label class="pl-5 " > วันคืนรถ : </label><input type="date" v-model="order_dateEnd"/>
       
            <!-- <h6 class="py-2">เลือกเวลา</h6> -->
            <label class="pl-5"> เวลารับรถ : </label><input type="time" v-model="order_timeStart"/>
            <label class="pl-5"> เวลาคืนรถ : </label><input type="time" v-model="order_timerEnd"/>
            
            <h3 class="py-2">สถานที่นัดรับ-คืนรถ</h3>
            <div >
                
                <div class="select">
                    <select class="select" name="location" v-model="order_detail">
                        <option  v-for="location in locations" v-bind:key="location._id">{{location.detail}}</option>
                    </select>
                </div>
            </div>
              
            </div>
            <div class="columns pt-3 is-10">
                <!-- <div class="column">
                     <router-link to="/">
                    <span class="icon is-large mr-6 mt-5" >
                        
                       <font-awesome-icon style="width:250px;height:25px" icon="fas fa-arrow-circle-left" />
                    </span></router-link>
                </div> -->
                <div class="column is-10"></div>
  
                <div class="column">
                    <div @click="Confirm()">
                        <!-- <router-link :to="{ name: 'showDetail', params: { car_id: this.order_carId }}"> -->
                    <span class="icon is-large ml-6 mt-5" >
                       <font-awesome-icon style="width:250px;height:25px" icon="fas fa-arrow-circle-right" />
                    </span>
                    <!-- </router-link> -->
                    </div>
                </div>
            </div>
            
           
              
           
            <div></div>
            
          
        </div></div>
        
 </div>
    
</template>


<script>
import axios from '../plugins/axios';


export default {
    
    data(){
        return{
            cars: [],
            test: true,
            //location-Date 
            order_detail:this.$route.params.location,
            order_timeStart:this.$route.params.timeStart,
            order_timerEnd:this.$route.params.timeEnd,
            order_dateStert:this.$route.params.dateStart,
            order_dateEnd:this.$route.params.dateEnd,
            //
            order_carType:this.$route.params.type,
            order_carBrand:"",
            order_carModel:"",
            order_carId:"",

            
            old_Model:"",
            car_type:[],
            // car_Brand:[],
            check_type:[],
            // check_Brand:[],
            getOnlyType:[],
            // getOnlyBrand:[],
            carByType:[],
            order_amountDay:"",
            order:[],
            
            oldCar:[],
            carId:this.$route.params.order_carId
            
           
           
        }
    },
    mounted(){
        this.getAllCars()
        this.selectType()
         this.getAllLocations()
         this.getAllBack()
         
        
    },
    methods: {
        getAllCars(){
            axios.get("/order/getAllCars")
            .then((res) => {
                this.cars = res.data
                this.check_type = res.data.map((val)=>{return val.type}).sort()
                this.car_type = this.check_type.map((val,index)=>{
                    if(val==this.check_type[index+1]&&index!==this.check_type.length-1){
                        return {"count":1,"type":val,"check":false}
                    }
                    else if(index==this.check_type.length-1 && val ==this.check_type[index-1] ){
                        return {"count":1,"type":val,"check":false}
                    }
                    
                    return {"count":0,"type":val,"check":false}
                    } )
                this.car_type.push({"count":0,"type":"All","check":false})
                this.getOnlyType = this.car_type.filter((val)=> val.count ===0)

                this.oldCar =  this.cars.filter((val)=>val.type==this.order_carType)

                    
               
            })
            .catch((err) => {
                console.log(err)
            })
        },
        getAllLocations(){
            axios.get("/order/getAllLocations")
            .then((res) => {
                this.locations = res.data
            })
            .catch((err) => {
                console.log(err)
            })
        },
         

       
        selectModel(id,model){ 
            this.check_model = true;
            this.order_carModel = model;
            this.carId = id;
            this.old_Model = model;
            this.order_carId = id
        },
         selectType(type){
            if(type ===""||type=="All"){
                this.carByType = this.cars
               
            }
            //แก้เพิ่ม
            else{
                this.carByType=this.cars.filter((val)=>val.type==this.order_carType && val.quantity >0);
                this.carId = ""
            }
            
        },
        // change(id){
        //     this.order_carId==id
        // },
        Confirm(){
            if(this.order_carId ==""){
                alert('กรุณาเลือกรถ');
            }
            else if(this.order_timeStart == ""|| this.order_timerEnd=="" || this.order_dateStert==""||this.order_dateEnd==""){
                alert('กรุณากรอกวัน-เวลาให้ครบ');
            }
            else if(this.order_detail==""){
                alert('กรุณาเลือกสถานที่ในการนัดรับรถ');
            }
             else {
                let month_start= parseInt(this.order_dateStert.slice(5,7))
                let month_end= parseInt(this.order_dateEnd.slice(5,7))
                let day_start= parseInt(this.order_dateStert.slice(8,11))
                let day_end= parseInt(this.order_dateEnd.slice(8,11))
           
                let month = parseInt(month_end-month_start)
                if(month_end<month_start){
                    month = (12-month_start) +month_end
                }
                console.log(day_start+"----m-s")
                console.log( day_end+"----m-e")
                if(month_end==month_start){
                    this.order_amountDay = day_end-day_start;
                    if(this.order_amountDay ==0){
                        this.order_amountDay =1
                    }
                    else if(this.order_amountDay <0){
                        alert('กรุณากรอกวัน-เวลาใหม่อีกรอบ')
                    }
                    else{
                        this.order_amountDay = day_end-day_start;
                    }
                    console.log(this.order_amountDay)

                }
                else{
                    this.order_amountDay = day_end+((30*month)-day_start)
                    console.log(this.order_amountDay)
                }
                this.page = 2;
                
                this.$router.push({name: 'showDetail',
                params:{car_id:this.order_carId,amountDay:this.order_amountDay,location:this.order_detail,dateStart:this.order_dateStert,dateEnd:this.order_dateEnd,timeStart:this.order_timeStart,timeEnd:this.order_timerEnd}
                 })
               
            } 
           
        },

    },
}
</script>

<style scoped>
</style>
