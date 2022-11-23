<template>
    <div>
      
         <div class="box"> <!--style="display: inline-block;"-->
                      <h3> {{burger.name}}</h3>
                      <img v-bind:src="burger.img"  style="height: 200px;">        
                      <ul>
                          <li>{{burger.kCal}} kCal</li>
                          <li v-if="burger.gluten">Contains <span class="allergies"> gluten</span> </li>
                          <li v-if="burger.soya">Contains <span class="allergies">soya</span></li>
                          <li v-if="burger.spicy"> This one is <span class="allergies">spicy</span> </li>
                      </ul>
                     <p><br><button v-on:click="addBurger()">+</button>{{ amountOrdered }}<button v-on:click="removeBurger()">-</button> </p>
                  </div>
       
                      
                     
     



    
  
       
    </div>
  </template>
  
  <script>
  
  let amountOrdered;
  let orderedBurger;
  export default {
    name: 'OneBurger',
    props: {
      burger: Object
    },
      data: function () {
      return {
        amountOrdered: 0,
               
      }
    },
    methods: {
      addBurger: function(){
        this.amountOrdered += 1;
                //console.log(amountOrdered);
        this.$emit('orderedBurger', { name:   this.burger.name, 
                                      amount: this.amountOrdered 
                                    }
        );
         orderedBurger={ name:   this.burger.name, 
                        amount: this.amountOrdered 
                      }; 
        console.log(orderedBurger);                          
      },
      removeBurger: function(){
        this.amountOrdered -= 1;
        this.$emit('orderedBurger', { name:   this.burger.name, 
                                      amount: this.amountOrdered 
                                    } 
        );
        orderedBurger={ name:   this.burger.name, 
                        amount: this.amountOrdered 
                      }; 
        console.log(orderedBurger);
      }

    }
  }
  
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
    .allergies {
      font-weight: bold;
      color: #3d251e;
    }
        .box {
          background-color: #fff;
          color: #000;
          border-radius: 1em;
          padding: 0.01em;
          font-size: 120%;
        }
       
   
      

  </style>
  