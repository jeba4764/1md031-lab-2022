<template>

    <div>
       <h3>{{ "box" }}</h3>
 
    
     </div>
     
   </template> 
 
 
 
   <script>
   export default {
     name: 'OneBox',
     props: {
       box: Object
     },
     // props: (obs är en property) 
     // props is a list of variables that are passed from parent components
     data: function() {
       return {
                 
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
   