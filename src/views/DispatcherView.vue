<template>
    <div id="orders">

      <div id="orderList">
        <div v-for="(order, key) in orders" v-bind:key="'order'+key"> <!--förstå den sista-->
          <!-- #{{ key }}: {{ order.orderItems.join(", ") }} -->
          #{{ key }}: {{ order.orderItems }} 
          <dd>{{order.customerInformation}}</dd>
        </div>
        <button v-on:click="clearQueue">Clear Queue</button>
      </div>

      <div id="dots" v-bind:style="{ background: 'url(' + require('../../public/img/polacks.jpg')+ ')' }">
        
          <!-- I vårt fall NÄR VI ANVÄNDE ADDORDER ME [beans, curry] har vi array of objects som  heter "objects"
               Som i sin tur kommer som ett object från servern

              { orders:
                { key:{} key:{} key:{} ... } 
              }
                    
              där varje {} är en order, ex { orderID: 123 
                                             details: { x: 45
                                                        y: 67 }
                                             orderitems:  ["beans, "curry"]
                                            } 
            -->
          <div  v-for="(order, key) in orders" 
                v-bind:style="{ left: order.details.x + 'px',
                               top: order.details.y + 'px'}"   
                v-bind:key="'dots' + key">                     
                
                <!--1a v-bind: ...uses v-bind to create dynamic styling of the element. Acesses datan details in the order och anvädner den-->
                <!--2a v-bind: Här används key som en internal key för:  "...vue to keep track of dynamically createt elements (with v-bind:key="dots + key) -->
                                             
            {{ key }}
          </div>
      </div>
      
    </div>
  </template>


  <script>
  import io from 'socket.io-client'
  const socket = io();
  
  export default {
    name: 'DispatcherView',
    data: function () {
      return {
        orders: null
      }
    },
    created: function () {
      socket.on('currentQueue', data =>
        this.orders = data.orders);
    },
    methods: {
      clearQueue: function () {
        socket.emit('clearQueue'); // dvs i socket:en clearas queuen!!
        
      }
    }
  }
  </script>


  <style>
  #orderList {
    top:1em;
    left:1em;
    position: absolute;
    z-index: 2;
    color:black;
    background: rgba(255,255,255, 0.5);
    padding: 1em;
  }
  #dots {
    position: relative;
    margin: 0;
    padding: 0;
    background-repeat: no-repeat;
    width:1920px;
    height: 1078px;
    cursor: crosshair;
  }
  
  #dots div {
    position: absolute;
    background: black;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
  }
  </style>
  