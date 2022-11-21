<template>
    <div class="burgeritem">

      <h5>{{ burger.name }} - {{ burger.burgerPrice }}  kr</h5> 
      
      <div class ="buttonContainer">
        <button v-on:click="removeItem(key)" id="removeButton">
        Ta bort
      </button>
      <p id="amountBurgers">
        Antal: {{amountOrdered}}
      </p>
      <button v-on:click="addItem(key)" id="addButton">
        Lägg till
      </button>
     
      </div>
     

      <img class ="burgerimage" v-bind:src = "burger.img" />

      <ul class="burgerlist">
        <li>{{burger.attribute1}}</li>
        <li>{{burger.attribute2}}</li>
        <li>{{burger.attribute3}}</li>
      </ul>
      <h3 id="allergyTitle">Innehåller:</h3>
     
      <ul class="burgerlist">
        <b><li v-if="burger.gluten">Gluten</li>
        <li v-if="burger.laktos">Laktos</li></b>
      </ul>
    </div>


  </template>
  
  <script>
  export default {
    name: 'OneBurger',
    props: {
      burger: Object
    },
    data: function(){
      return {
        amountOrdered: 0,
      }
    },
    methods: {
      addItem: function() {
        this.amountOrdered ++;
        this.$emit('orderedBurgers', { name:   this.burger.name, 
                                      amount: this.amountOrdered 
                                    }
                  );
      },
      removeItem: function() {
        if(this.amountOrdered > 0){
          this.amountOrdered --;
          this.$emit('orderedBurgers', { name:   this.burger.name, 
                                      amount: this.amountOrdered 
                              }
        );
        }
      }
    }
  }

  
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
  .burgeritem {
    justify-items: center;
    padding: 15px;
    
   

}
.burgertitle {
    justify-content:;
}

.burgerlist {
    display: flex;
    flex-direction: column;
    justify-content: center;
    margin-left: 50px;

}

.burgerimage {
    height: 200px;
    width: auto;
    justify-content:center;
    border: 5px ridge white;
    
}

.ingridient{
font-weight: bold;
}

.buttonContainer {
  display: flex;
  justify-content: center;
  text-align: center;
  margin-top: 10px;
  margin-bottom: 10px;
  
}

#addButton {
  background-color: green;
  color: white;
  justify-content: center;
  margin-bottom: 5px;
  width: 75px;
  border-radius: 5px;
  border: 1px solid white;
}

#addButton:hover {
  background-color: rgb(16, 168, 16);
  transition: 0.2s;
  color: black;
}
#removeButton {
  background-color: red;
  color: white;
  width: 75px; 
  justify-content: center;
  margin-bottom: 5px;
  border-radius: 5px;
  border: 1px solid white;
}

#removeButton:hover {
  background-color: rgb(251, 119, 119);
  transition: 0.2s;
  color: black;
}

#amountBurgers {
margin: 0px;
padding-left: 10px;
padding-right: 10px;
}

#allergyTitle {
text-align: left;
margin-left: 70px;
font-weight: 100;
margin-top: 50px;
}
  </style>
  