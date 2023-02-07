<template>
    <div>   
    
      <header>
          <h1>Välkommen till BurgerOnline</h1>
          <img src="https://t3.ftcdn.net/jpg/03/08/47/58/360_F_308475881_m7o04LTdIbwWWnSJDqjFTjOnWvU0yj8b.jpg">
      </header>
  
      <main>
          <section id="idVäljburgare">
              <h2>Välj burgare</h2>
              <p>Här väljer du burgare</p>
  
              <ul>
                  <!-- <li v-for="(item, key, index) in objectItems">{{ item.pos }} {{key}} {{index}}</li> -->
                  <!-- {{objectItems[0].pos}} -->
              </ul>
                
  
  
              <div class="wrapper">
  
                  <Burger v-for="burger in burgers"
                              v-bind:burger="burger"  
                              v-bind:key="burger.productName"
                              v-on:orderedBurger="addToOrder($event)"/>                        
                            
              </div>
  
          </section>
  
          <section id="idKundinformation">
              <h2>Kundinformation</h2>
              <p>Här ger du nödvändig information om dig som kund </p>
  
                  <div>
                      <h3>Leveransinformation:</h3>
  
                      <p>
                          <label for="För- och efternamn">För- och efternamn</label>
                          <br>
                          <input type="text" id="För- och efternamn" v-model="firstLastName" required ="required" placeholder= "För- och efternamn">
                      </p>
  
                      <p>
                          <label for="Email">Email</label>
                          <br>
                          <input type="email" id="Email" v-model="email" required ="required" placeholder="Email">
                      </p>
  
                  </div>
  
  
                  <div>                        
                      <label for="Betalningsmetod">Betalningsmetod</label>
                      <br>
                      <select id="Betalningsmetod" v-model="betalningsmetod">
                          <option>Kontokort</option>
                          <option>Bank</option>
                          <option>Klarna</option>
                          <option>Swish</option>
                      </select>                        
                  </div>
  
                  <br>
                  <div>
                      <p>Köstillhörighet:</p>
  
                      <div>
                          <label for="Vill ej ange">Vill ej ange</label>
                          <input type="radio" id="Vill ej ange" v-model="könstillhörighet" value="Vill ej ange" checked="checked">
                      </div>
  
                      <div>
                          <label for="Kvinna">Kvinna</label>
                          <input type="radio" id="Kvinna" v-model="könstillhörighet" value="Kvinna">
  
                      </div>
  
                      <div>
                          <label for="Man">Man</label>
                          <input type="radio" id="Man" v-model="könstillhörighet" value="Man">
                      </div>
  
                      <div>
                          <label for="Annan">Annan</label>
                          <input type="radio" id="Annan" v-model="könstillhörighet" value="Annan">
                      </div>                        
                  </div>
                                                                  
          </section>
      
  
          <div id="mapDiv">
  
              <div id="map" v-on:click="setLocation">

                  <div v-bind:style="{ left: location.x + 'px', 
                                       top: location.y + 'px' }">
                          T
                  </div>            
              </div>
          </div>
  
      </main>
  
      <div>
          <button type="submit" v-on:click="orderButton">
              <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcR1MwbVW0aHcPgLaoprBh3cj68r067r_uW17w&usqp=CAU" width="200px;">
              <p>Skicka beställning</p>
          </button>
      </div>
  
  
      <hr>
      <footer>       
     
      </footer>
  
    </div>  
  </template>
  
  
  
  
  <script>
  import Burger from '../components/OneBurger.vue'
  import io from 'socket.io-client'
  import menu from '../assets/menu.json'
  
  const socket = io();
  const menuItems = menu;
  
  // function MenuItem (pn, ui, kCal, g, l) {
  //     this.productName = pn; 
  //     this.urlImg = ui ;
  //     this.kCal = kCal;
  //     this.gluten = g;
  //     this.lactose = l;
  //     this.name = function() {
  //         return ;
  //      };
  //  }
   
  
  export default {
    name: 'HomeView',
  
    components: {
      Burger
    }, 
  
    data: function () {
      return {
          
          objectItems: {
          key1: {namn:'item1', pos: 12}, 
          key2: {namn:'item2', pos: 34}, 
          key3: {namn:'item3', pos: 56}, 
        },
        orderedBurgers: {},      
        burgers: menuItems,
        location: {x: 0,
                   y: 0
                  },
  
        firstLastName: '',
        email: '',
        betalningsmetod: '',
        könstillhörighet: ''          
        }  
    },
  
    methods: { 
  
      getOrderNumber: function () {
        return Math.floor(Math.random()*100000);
      },
  
      // addOrder: function (event) {
      //   var offset = {x: event.currentTarget.getBoundingClientRect().left,
      //                 y: event.currentTarget.getBoundingClientRect().top};
      //   socket.emit("addOrder", { orderId: this.getOrderNumber(),
      //                             details: { x: event.clientX - 10 - offset.x,
      //                                        y: event.clientY - 10 - offset.y },
      //                             orderItems: ["Beans", "Curry"]
      //                           }
      //             );
      // this.location.x = event.clientX - 10 - offset.x;                        
      // this.location.y = event.clientY - 10 - offset.y;
      // },
  

  
      setLocation:function(event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
  
      this.location.x = event.clientX - 10 - offset.x;                        
      this.location.y = event.clientY - 10 - offset.y;   
   },
  
      orderButton:function() {    
      socket.emit("addOrder", {orderId: this.getOrderNumber(),                     
                               details: {x: this.location.x,
                                        y: this.location.y},
                               orderItems: this.orderedBurgers,
                               customerInformation: [this.firstLastName, this.email, this.betalningsmetod, this.könstillhörighet]
                              }                  
      );     
      console.log(this.firstLastName, this.email, this.betalningsmetod, this.könstillhörighet ) ;
      console.log(this.orderedBurgers);
      // console.log({orderId: this.getOrderNumber(),                     
      //                         details: {x: this.location.x,
      //                                   y: this.location.y},
      //                         orderItems: this.orderedBurgeres,
                                 //  customerInformation: [this.firstLastName, this.email, this.betalningsmetod, this.könstillhörighet]
                           //   }  );
  
  
      
      // socket.emit("addOrder", {orderId: this.getOrderNumber(),                     !!!!!!
      //                          details: {x: this.location.x,
      //                                    y: this.location.y
      //                                 },
      //                          orderItems: this.orderedBurgeres,
      //                          caustomerInformation: [this.firstLastName, this.email, this.betalningsmetod, this.könstillhörighet]
      //                         }
      //             );
  
      },
  
      addToOrder: function(event) {
          this.orderedBurgers[event.name] = event.amount;
          // för varje Burger sparas burger name som key me antal burgare som values, ex hästburgare:2
      },
    }
  
  }
  </script>
  
  
  
  <style>
  
  body {
      font-family: 'Lucida Sans', 'Lucida Sans Regular', sans-serif;
      font-size: 110%;
  }
  
  header  {    
      margin: 0 1.2em;
      height: 14em;
      width: 100%;
      overflow: hidden;
      }
  
 
  
  header img {
      opacity: 0.55;
      margin-top: -3em;
      
      }
  
  header h1 {
      position: absolute;
      margin: 1em 0 0 0.7em;
  
  /* HUR FIXA SÅ TTITEL EJ ÄNDRAS MED STORLEK, NÄR JAG 
  GÖR SOM NEDAN BLIR DET FULT */
  
      /* width: 100%;
      overflow: hidden; */    
  }
  
  .bolded {
      font-weight: bold;
  }
  
  .wrapper {
      display: grid;
      grid-gap: 10px;
      grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  
      background-color: rgb(30, 29, 29);
      color: white;  
      }
  
  
   #idVäljburgare {
      background-color: rgb(30, 29, 29);
      color: white;   
  
      padding: 1.2em;
      border: 0.2em dashed white;
      margin: 1.2em;
      }
  
  
  #idKundinformation {
      border: 0.2em dashed black;
      margin: 1.2em;
      padding: 1.2em;
  }
  
  
  
  button {
      
      margin: 1.3em;
      font-family: 'Lucida Sans', 'Lucida Sans Regular', sans-serif;
      font-size: 100%;
  }
  
  
  button:hover {
      background-color: rgb(143, 143, 143);
      cursor:pointer;
  }
  
   #mapDiv {
      margin-left:1.2em;
      width: 550px;
      height:450px;
      overflow:scroll;
      position: relative;
   }
  
    #map {
      width: 1920px;
      height: 1078px;
      background: url( "../../public/img/polacks.jpg");
   
      cursor: pointer;
      position: absolute;
   }
    
    #map div {
      position: absolute;
      background: black;
      color: white;
      border-radius: 10px;
      width: 20px;
      height: 20px;
      text-align: center;
    }
  
  </style>
  
  
  