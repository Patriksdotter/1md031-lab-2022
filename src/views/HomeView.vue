<template>
<div>
 
  <!-- <body> --> 
  <header>
            <div id="headlinediv">
            <h1 id="headline">That Vegan Place</h1> 
            <img id="headerimage" src="https://images.hindustantimes.com/img/2021/10/31/1600x900/world_vegan_day_thumb_1635693633164_1635693643392.jpg">
            </div>
            
        </header>
        <main>
       
            <section id="selectburger">
                <h2>Select burger</h2>
                <p>Choose your burger!</p>
                
                <div class="wrapper">
                <Burger v-for="burger in burgers"
              v-bind:burger="burger" 
              v-bind:key="burger.name"
              v-on:orderedBurger="addToOrder($event)"/>
                  
                 
                </div>

            </section >
            <section id="customerinformation" >
                <h3> Customer information</h3>
                <p>This is where you provide necessary information</p>
                <h4> Delivery information:</h4>
                <div id="contact">
                    <form>
                    
                        <p>
                            <label for="firstandlastname">Full Name {{fulln}}</label><br>
                            <input type="text" id="firstandlastname" v-model="fulln" required="required" placeholder="First and Last name">
                        </p>
                        <p>
                            <label for="email">Email</label><br>
                            <input type="email" id="email" v-model="email" required="required" placeholder="E-mail address">
                        </p>

                         <p> Click on the map to determine the delivery address </p>
                          <div id="mapdiv" >
                                  <div id="map" v-on:click="setLocation" >
                                   <div id="dots" v-bind:style="{}">
                                   <div v-bind:style="{ left: location.x + 'px', 
                                                            top: location.y + 'px'}" v-bind:key="'dots' + key" >
                                        T
                                      </div>  
                                      </div>
                                    </div>
                                  </div> 
                        <p>
                            <label for="paymentmethod">Payment method:</label>
                            <select id="paymentmethod" v-model="paymentmethod">
                                <option>Swish</option>
                                <option>Debit card/Credit card</option>
                                <option>Paypal</option>
                                <option>BitCoin</option>
                                <option>Cash</option>
                            </select>
                         </p>
                         <p>Gender <br>
                            <input type="radio" id="female" v-model="gender" value="female">
                            <label for="female">Female </label><br>
                            <input type="radio" id="male" v-model="gender" value="male">
                            <label for="male">Male</label><br>
                            <input type="radio" id="nonbinary" v-model="gender" value="nonbinary">
                            <label for="nonbinary">Non-binary</label><br>
                            <input type="radio" id="undisclosed" v-model="gender" value="undisclosed">
                            <label for="undisclosed">Undisclosed</label>
                         </p>
                         
                    </form>
                </div>
            </section>
            
                                             
                          
             
            <button v-on:click="addOrder()" type="submit">
             <img src="https://www.shareicon.net/data/512x512/2015/10/03/111191_food_512x512.png" alt="Ordering button Icon" title=" Icon" style="width: 15px;">
                Place order!
            </button>
           
        </main>
        <hr>
        <footer>
            &COPY; 1943 The Vegan Neighbour Inc.
        </footer>
<!-- </body> -->
</div>
</template>

<script>

import Burger from '../components/OneBurger.vue'
import menu from '../assets/menu.json'
import io from 'socket.io-client'
let theMenu = menu;
//const hej = "hej"
console.log(theMenu);

const socket = io();
//console.log(menu);
if (typeof theMenu !== 'object'){
  theMenu = JSON.parse(theMenu);
}

/* function MenuItem(name, img, g, so, spicy, kCal) {
    this.name = name; // The *this* keyword refers to the object itself
    this.image = img;
    this.gluten = g;
    this.soya = so;
    this.spicy = spicy;
    this.kCal= kCal;
} */

let arrayBurgers = theMenu;
let orderedBurgers={}
//console.log(arrayBurgers);
export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: arrayBurgers,
      orderedBurgers: {},
      location: { x: 0,
                  y: 0
                },
      name: '',
      customerEmail: '',
      payment: '',
      customerGender: '',
      paymentmethod: 'Swish',
      
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    getLocation: function(event) {
      this.location = {x: event.clientX,
                       y: event.clientY
                      }
    },
    
    setLocation: function(event){
     var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      this.location.x = event.offsetX- 30 ;
      this.location.y= event.offsetY - 30;  
    },
    addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;
      console.log(orderedBurgers);
    },
    addOrder: function(event){
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: this.location.x,
                                          y: this.location.y },
                                orderItems: this.orderedBurgers,
                                customerInfo: {name: this.fulln,
                                               customerEmail: this.email,
                                               payment: this.paymentmethod,
                                               customerGender: this.gender}
                              }
                              
                );
      
      console.log(this.location.x);
     console.log(this.fulln);
      console.log(this.email);
      console.log(this.paymentmethod);
      console.log(this.gender);
      console.log(this.orderedBurgers);
    }
    
  }
}
</script>

<style>
@import 'https://fonts.googleapis.com/css?family=Pacifico|Dosis';
@media screen and (max-width: 800px) {
    h1 {
        font-size: 6vw;
    }
}


body {
    font-family: "Courier New", monospace;
    font-size: 1.2em;
    /*font-style: italic;*/
    color: #000;
 }
 .wrapper {
    display: grid;
    grid-gap: 0.1em;
    grid-template-columns: 33% 33% 33%;
    background-color: #fff;
    color: #444;
}

 #customerinformation {
    background-color: #000;
    color: #FFFFFF;
 }
 button:hover {
    background-color: #87ab69;
    cursor: pointer;
 }
 section{
    margin: 1.5em 1em;
    border: 0.7em double currentColor;
    
 }
 section p , h2, h3, h4{
    margin: 1em 1em;
 }

 div{
    margin: 1em 1em;
 }
 button{
       margin: 1em 1em 1em 2em;
 }
 #headlinediv{
    height: 9em;
    overflow:hidden;
 }
 
 #headline{
    position: absolute;
    padding: 1em;
    margin-top: -1;
    
 }
 #headerimage{
    opacity: 0.4;
    height: auto;
    width: 100%;
 }

  #map {
    width: 1920px;
    height: 1078px;
    background-image: url('../../public/img/polacks.jpg');
    cursor:crosshair;
    position: relative;
    margin: 0;
    padding: 0;
    background-repeat: no-repeat;
    
  }
  #mapdiv{
    height: 25em;
    width: 97%;
    overflow: scroll;
  }
  
 
  #dots div{
    position: absolute;
    background: black;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
  }

</style>