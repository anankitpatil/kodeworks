<template>
<div class="container">
  <div class="toggler">
    <div class="form-col">
      <label>Restaurant name</label>
      <select v-model="restaurant">
        <option value="" selected>Choose Restaurant</option>
        <option v-for="rst in myJson.data" :value="rst.name">{{ rst.name }}</option>
      </select>
      <span></span>
    </div>
    <div class="form-col">
      <label>Veg / Non Veg</label>
      <select v-model="veg">
        <option value="-1" selected>Choose Item</option>
        <option value="1">Veg</option>
        <option value="0">Non Veg</option>
      </select>
      <span></span>
    </div>
    <div class="form-col">
      <label>Cuisines</label>
      <select v-model="cuisine">
        <option value="" selected>Choose Item</option>
        <option v-for="cui in cuisines" :value="cui">{{ cui }}</option>
      </select>
      <span></span>
    </div>
    <div class="form-col">
      <label>Average cost for 2</label>
      <input type="number" placeholder="Add price" v-model="price" />
    </div>
    <div class="form-col">
      <button @click="filter">FILTER</button>
    </div>
  </div>
  <div class="items">
    <a href="#" class="item" v-for="item in items">
      <div class="image" :style="{ backgroundImage: `url('${item.image}')` }"></div>
      <div class="details">
        <div class="icon" :class="{non : item.isVeg == 0}"></div>
        <div class="text">{{item.name}} <span>Indian, Fast food</span></div>
      </div>
      <div class="rating" :class="{orange : item.rating <= 3}">
        <svg id="svgelem" viewBox="0 0 190 190" xmlns="http://www.w3.org/2000/svg">
          <polygon points="100,10 40,180 190,60 10,60 160,180" fill="white" />
        </svg>
        <span>{{item.rating}}</span>
      </div>
      <div class="price">&#8377; {{item.cost}}/- FOR 2</div>
    </a>
  </div>
</div>
</template>

<script>
import json from '../source.json'
export default {
  name: 'HelloWorld',
  data() {
    return {
      msg: 'Welcome to Your Vue.js App',
      myJson: json,
      restaurant: '',
      veg: 1,
      cuisine: '',
      cuisines: [],
      price: 0,
      items: []
    }
  },
  mounted() {
    var self = this
    // List unique cuisines
    json.data.forEach(function(data) {
      self.cuisines = self.arrayUnique(self.cuisines.concat(data.cuisines))
    })
    // List all menu items
    json.data.forEach(function(data) {
      data.menu.items.forEach(function(item) {
        item.image = data.imgUrl
        item.rating = data.avgRating
        item.cost = data.avgCost
        self.items.push(item)
      })
    })
  },
  methods: {
    filter: function() {
      var self = this
      self.items = []
      json.data.forEach(function(data) {
        data.menu.items.forEach(function(item) {
          item.image = data.imgUrl
          item.rating = data.avgRating
          item.cost = data.avgCost
          // Filter name of restaurant
          if (data.name == self.restaurant || self.restaurant == '') {
            // Filter veg/non veg
            if (item.isVeg == self.veg || self.veg == -1) {
              // Filter cuisine
              if (data.cuisines.indexOf(self.cuisine) > -1 || self.cuisine == '') {
                // Filter cost for 2
                if (item.cost <= self.price || self.price == 0) {
                  self.items.push(item)
                }
              }
            }
          }
        })
      })
    },
    arrayUnique: function(array) {
      var a = array.concat();
      for (var i = 0; i < a.length; ++i) {
        for (var j = i + 1; j < a.length; ++j) {
          if (a[i] === a[j])
            a.splice(j--, 1);
        }
      }
      return a;
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container {
  max-width: 1200px;
  width: 100%;
  margin: 0 auto;
}

.toggler {
  overflow: auto;
  padding-bottom: 30px;
  border-bottom: 2px solid #e5eaed;
}

.toggler .form-col {
  width: 20%;
  float: left;
  text-align: left;
  font-size: 12px;
  color: #778699;
  position: relative;
}

.toggler .form-col input,
.toggler .form-col select {
  width: calc(100% - 30px);
  -webkit-appearance: none;
  background: #FFF;
  line-height: 18px;
  font-size: 15px;
  padding: 6px;
  border: 1px solid #ecf0f3;
  border-radius: 2px;
  color: #c6cbd1;
}

.toggler .form-col select {
  width: calc(100% - 15px);
}

.toggler .form-col span {
  position: absolute;
  right: 24px;
  bottom: 10px;
  border: 5px solid transparent;
  border-top: 5px solid #aab8bb;
}

.toggler .form-col button {
  width: 100%;
  -webkit-appearance: none;
  background: #ffbe4e;
  border: none;
  color: #FFF;
  border-radius: 2px;
  font-size: 15px;
  line-height: 25px;
  margin-top: 19px;
}

.items {
  padding-top: 30px;
  overflow: auto;
}

.items .item {
  width: calc(25% - 22.5px);
  margin-right: 30px;
  margin-bottom: 30px;
  padding-bottom: 18px;
  float: left;
  display: block;
  background: #FFF;
  text-align: left;
  text-decoration: none;
  box-shadow: 0 0 6px rgba(0, 0, 0, .1);
  transition: 0.3s ease;
}
.items .item:hover {
  box-shadow: 0 0 9px rgba(0, 0, 0, .3);
}

.items .item:nth-child(4n) {
  margin-right: 0;
}

.items .item .image {
  width: 100%;
  display: block;
  padding-bottom: 60%;
  background-size: cover;
  background-position: center center;
}

.items .item .details {
  padding: 12px;
  border-top: 2px solid #f5f9fa;
}

.items .item .details .icon {
  display: inline-block;
  width: 12px;
  height: 12px;
  border-radius: 50%;
  margin-right: 6px;
  border: 2px solid #e2eaec;
  background: #65bb8a;
  vertical-align: top;
}

.items .item .details .icon.non {
  background: #f66457;
}

.items .item .details .text {
  width: calc(100% - 26px);
  display: inline-block;
  font-weight: 500;
  text-align: left;
  line-height: 15px;
  color: #6d7c91;
}

.items .item .details .text span {
  display: block;
  font-size: 12px;
}

.items .item .rating {
  background: #3bc675;
  border-radius: 2px;
  margin-left: 12px;
  padding: 6px 12px 3px 6px;
  display: inline-block;
}

.items .item .rating.orange {
  background: #ffa863;
}

.items .item .rating svg {
  width: 15px;
  height: 15px;
  margin-right: 6px;
  display: inline-block;
}

.items .item .rating span {
  display: inline-block;
  vertical-align: top;
  font-size: 14px;
  color: #FFF;
  line-height: 22px;
}

.items .item .price {
  float: right;
  font-size: 14px;
  color: #6d7c91;
  margin-right: 12px;
  padding-top: 8px;
}
</style>
