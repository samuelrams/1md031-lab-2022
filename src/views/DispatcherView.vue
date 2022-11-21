<template>
   
    <div id="orders">
      <div id="orderList">
        <div v-for="(order, key) in orders" v-bind:key="'order'+key">
          <b>Order ID:</b> #{{order.orderId}}:
          <div id="burgerItemList" v-for="(item) in order.orderItems" v-bind:key="'item'+key">
            <div v-for="(amount, name) in item" v-bind:key="'name'+key">
           <p id ="burgeritems" v-if="amount!=0">
            {{name}}: {{amount}}
          </p>
           </div>
          </div>
          <div id="personalInfo">
          <i><b>{{order.details.name}}</b> 
          ({{order.details.email}}, 
          {{order.details.gender}}, 
          {{order.details.payment}})</i>
          <hr>
          </div>

        </div>
        <button v-on:click="clearQueue" id="clearbutton">Clear Queue</button>
      </div>
      <div id="dots" v-bind:style="{ background: 'url(' + require('../../public/img/polacks.jpg')+ ')' }">
          <div v-for="(order, key) in orders" v-bind:style="{ left: order.details.x + 'px', top: order.details.y + 'px'}" v-bind:key="'dots' + key">
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
        socket.emit('clearQueue');
      }
    }
  }
  </script>
  <style>
  #orderList {
    text-align:left;
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

  #clearbutton {
    background-color: white;
    border-radius: 15px;
    margin-top: 10px;
  }

  #clearbutton:hover {
    background-color: rgb(223, 186, 186);
    color: white;
    transition: 0.2s;
    border: 2px solid rgb(128, 0, 41);
    
  }

  #name {
    font-weight: bold; 
  }

  #burgeritems {
    margin-bottom: 0px;
    margin-top: 0px;
  }

  #burgerItemList {
    margin-top: 5px;
    margin-bottom: 5px;
  }

  #personalInfo{
    font-size: 16px;
    margin-top: 10px;
  }
  </style>
  