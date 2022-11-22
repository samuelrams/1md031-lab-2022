<template>
  <div>
 

  
    <header>  
        <img src ="https://images.unsplash.com/photo-1570838987781-cfb11bb0a4e4?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1035&q=80" id="headerimage"> 
        <h1 id = "pagetitle">
            Samuels Burgarhak
        </h1> 
    </header>
   
    <main>
        <section id="burgersection">
            <div>
            <h3 class="burgertitles">
                Välj din burgare
            </h3>
            <p>
                Vad är du sugen på? Välj din burgare här!
            </p>
            </div>
            <div class="burgerwrapper">
             <Burger v-for="burger in burgers"
              v-bind:burger="burger" 
              v-bind:key="burger.name"
              v-on:orderedBurgers="addToOrder($event)"/>
            </div> 
        </section>
        <div class="bottomHalfPage">
        <section id="contact">
            <form id="form">
              <h3>Leveransinformation</h3>
            <p>
              Vänligen fyll i dina uppgifter för leverans:
            </p>
                <p>
                    <label for="fullname">Namn</label><br>
                    <input type="text" id = "fullname" v-model="fn"  required="required" placeholder="För- och efternamn">
                </p>
                <p>
                    <label for="email">E-mail</label><br>
                    <input type="email" id = "email" v-model="em" required="required" placeholder="Mailadress">
                </p>
                <p>
                    <label for="payment">Betalningsalternativ:</label><br>
                    <select v-model="payment" id="payment" name="pa">
                        <option selected="selected">Swish</option>
                        <option>Kort</option>
                        <option>Guld</option>
                        <option>Cash</option>
                    </select>
                 </p>
                 <div>
                  <p>Klicka i kartan för att markera din leveransadress:</p>
                  <div class="mapContainer">
      <div id="map" v-on:click="setLocation" v-bind:style="{background: 'url(' + require('/Users/samuelram/Documents/Uppsala Universitet/Kurser/Gränssnittsprogrammering/1md031-lab-2022/public/img/polacks.jpg')+ ')' }">
        <div id="dots" v-bind:style="{ left: location.x + 'px', top: location.y + 'px'}" v-bind:key="'dots' + key">
        Ø
      </div>
  </div>
</div>

            <h3>Personlig information</h3>
            
                 <p id="genderP">
                   
                        <label>Kön:</label><br>
                           
                            <input type="radio" id="woman" value="Kvinna" name="drone" v-model="gender" />
                            <label for="woman">Kvinna</label><br>
                       
                            <input type="radio" id="man" value="Man" name="drone" v-model="gender" />
                            <label for="man">Man</label><br>
                       
                            <input type="radio" id="other" value="Annat" name="drone" v-model="gender" />
                            <label for="other">Annat</label><br>
                        
                            <input type="radio" id="not" value="Vill inte ange" name="drone" v-model="gender" />
                            <label for="not">Vill inte ange</label>

                            
                             
                 </p>
                 </div>

                <h3 class="formh3">Klar?</h3>
                <p>Klicka på knappen nedan för att beställa!</p>
                <button id="orderButton" v-on:click="finishOrder(key)" type="submit">
            <img src ="https://cdn-icons-png.flaticon.com/512/130/130066.png" id="bikeicon">
            <span id="ordertext">Beställ</span>
        </button>
            </form>
        </section>
        <!--<section>
                  <img src="https://images.unsplash.com/photo-1564019709518-6182bdabe251?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxzZWFyY2h8NHx8YnVyZ2VyJTIwcmVzdGF1cmFudHxlbnwwfHwwfHw%3D&auto=format&fit=crop&w=800&q=60" id="nicePic">
                 </section>-->
        </div>
        <div>
        
      </div>
     
    </main>
    <hr>
    <footer>
        &COPY; 2022 Sampes Burgare Inc.
    </footer>
  </div>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io();

function MenuItem (nm, im, price, allergene){
  this.name = nm;
  this.img = im;
  this.burgerPrice = price;
  this.allergenes = allergene;
}

let burger1 = new MenuItem("McNils", "https://insanelygoodrecipes.com/wp-content/uploads/2020/10/Hamburger-with-Sesame-Seeds-Cheese-and-Veggies-500x375.png", "139", "Nej");
let burger2 = new MenuItem("McIsak", "https://assets.biggreenegg.eu/app/uploads/2019/03/28145521/topimage-classic-hamburger-2019m04-800x534-600x401.jpg", "149", "Ja");
let burger3 = new MenuItem("McFelix", "https://www.nj.com/resizer/IKzHhfYmO_TN3M-SpmuEuzWZbGU=/arc-anglerfish-arc2-prod-advancelocal/public/SWZEWO7EQFCNTDOPCOOON62CEE.JPG", "23", "Ja");
let myBurgers = [burger1,burger2,burger3]

console.log(myBurgers);

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: menu,
      orderedBurgers: {},
      location: {x: 0,
                 y: 0
                },
      gender: '',
      payment: '',
          
    }
  },
  methods: {

  
    addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;
    },
    finishOrder: function() {
      console.log(this.fn)
      console.log(this.em)
      console.log(this.orderedBurgers)
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: this.location.x,
                                           y: this.location.y,
                                          name: this.fn,
                                          email: this.em,
                                          gender: this.gender,
                                          payment: this.payment},
                                          orderItems: [this.orderedBurgers]
                              }
                 );
    },
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      this.location.x = event.clientX - 10 - offset.x;
      this.location.y = event.clientY - 10 - offset.y;
     
    },

    setLocation: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      this.location.x = event.clientX - 10 - offset.x;
      this.location.y = event.clientY - 10 - offset.y;
    }
    
  }
}
</script>

<style>

@font-face {
  font-family: "soft-marshmallow";
  src: url('/Users/samuelram/Library/Fonts/Soft Marshmallow.otf');
  src: url('/Users/samuelram/Library/Fonts/Soft Marshmallow.otf') format('opentype');
}

@font-face {
  font-family: "parisish";
  src: url('/Users/samuelram/Library/Fonts/Parisish.ttf');
  src: url('/Users/samuelram/Library/Fonts/Parisish.ttf') format('truetype');
}

@font-face {
  font-family: "no-virus";
  src: url('/Users/samuelram/Library/Fonts/No Virus.ttf');
  src: url('/Users/samuelram/Library/Fonts/No Virus.ttf') format('truetype');
}

@font-face {
  font-family: "borgens-burl";
  src: url('/src/assets/Fonts/Borgens Burlesque.ttf');
  src: url('/src/assets/Fonts/Borgens Burlesque.ttf') format('truetype');
}
  
  body {
    font-family: courier;
    font-size: 1.2rem;
    background-color: #929292;
    text-align: center;
}

h1 {
  font-family: borgens-burl;
  font-weight: 100;
  font-size: 120px;
  color: black;
}

.burgertitles {
  font-family: borgens-burl;
  font-weight: 100;
  font-size: 45px;
  margin-bottom: -5px;
  margin-top: 20px;
}

h5 {
  font-family: borgens-burl;
  font-weight: 100;
  font-size: 40px;
  margin-bottom: 0px;
  margin-top: 10px;
}

header {
    display: flex;
    align-items: center;
    justify-content: center;
    margin: 20px;
    height: 250px;
    overflow: hidden;

}

#headerimage {
    position: relative;
    top:80%;
    height: auto;
    width: 100%;
    opacity: 0.4;
}


#pagetitle {
position: absolute;
margin: 0px;

 
}

.ingridient{
font-weight: bold;
}

button:hover, #payment:hover, #man:hover, #woman:hover, #other:hover, #not:hover {
    cursor:pointer;
    
}

button:hover {
  background-color:rgb(104, 192, 97);
  transition: 0.2s
}

section {
    margin: 20px;
    padding: 10px 30px;
    
}

#burgersection {
    background-color: rgb(100, 100, 100);
    color: white;
    border: 5px solid black;
    height: auto;
    
}


#contact {
    border: 5px solid black;
    padding-right: 100px;
    text-align: left;
    
    background-color: rgb(196, 213, 198);
    
}

button {
    background-color: rgb(163, 230, 163);
    display:flex;
    margin: 0px 0px 0px 0px;
    font-size: 16px;
    align-self: flex-start;
    border-radius: 10px;


}

.burgerwrapper {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    
}


.burgeritem {
    justify-items: center;
    padding: 15px;
    
   

}

.burgertitle {
    justify-content:;
}

.burgerlist {
    justify-content: ;
}

.burgerimage {
    width: 180px;
    justify-content:;
    
}

#mcisakimage {
    width: 200px;
}

#bikeicon {
    width: 20px;
    margin-right: 4px;
    margin-bottom:3px;
    margin-top: 4px;
}

#ordertext {
    margin-top: 5px;
}

li {
    text-align: left;
}

/*.bottomHalfPage {
  display: grid;
  grid-template-columns: 2fr 1fr;

}*/

#nicePic {
  width: 700px;
  height: auto;
  opacity: 0.6;
 
}

footer {
  text-align: left;
  margin-left: 20px;
}
.mapContainer {
  margin-top: -10px;
  margin-bottom: 20px;
  height: 600px;
  width: 1000px;
  overflow: scroll;
  border: 2px solid white;
  
}
#map {
    position: relative;
    background-repeat: no-repeat;
    width:1920px;
    height: 1078px;
    cursor: crosshair;
  }
  
  #map div {
    position: absolute;
    background: black;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
  }

#genderP {
  margin-top: -10px;
}

.formh3 {
  margin-bottom: -10px;
}

#orderButton {
  margin-bottom: 10px;
  font-family: arial;
  font-weight: bold;
}
</style>