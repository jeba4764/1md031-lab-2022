<template>

   <div>
      <h3>{{ burger.productName }}</h3>

      <img id="burgPic" v-bind:src="burger.urlImg" >
        <ul>
            <li v-if="burger.productName=='Hästburgare'"> Närproducerat kött</li>
            <li v-if="burger.productName=='Korv med bröd'"> Som en burgare fast med korv och korvbröd </li>
            <li v-if="burger.gluten">Innehåller <span class="bolded">gluten</span></li>
            <li v-if="burger.lactose">Innehåller <span class="bolded">laktos</span></li>
        </ul>
        
      <p>
        Antal valda: {{amountOrdered}}
        <button type="button" v-on:click="removeBurger()">-</button>
        <button type="button" v-on:click="addBurger()">+</button>
      </p>      
      

    </div>
    
  </template> 



  <script>
  export default {
    name: 'OneBurger',
    props: {
      burger: Object
    },
    // props: (obs är en property) 
    // props is a list of variables that are passed from parent components
    data: function() {
      return {
        amountOrdered: 0,
        
      }
    },

    methods: {
      addBurger: function() {
        this.amountOrdered += 1;
        this.$emit('orderedBurger', {name: this.burger.productName,
                                      amount: this.amountOrdered
                                    }
        );
      },
      removeBurger: function() {
        this.amountOrdered -=1;
        this.$emit('orderedBurger', {name: this.burger.productName,
                                      amount: this.amountOrdered
                                    }
        );
      }
    }

  }
  </script>

  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>

  #burgPic {
    width: 12em;
    height: 8em;
  }

  button {
    width: 30px;
    
  }
  
  </style>
  