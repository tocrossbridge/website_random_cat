<template>
  <div class="row">

    <div class="col d-inline-block">
      <input v-model="cats.newCat" placeholder="Write the name of a kitty here :D" />
      <button v-on:click="addKitty" class="btn w-100">Add a kitty</button>
  
      <ul>
        <li v-for="(cat, index) in cats.list" :key="index">{{ cat.name }}</li>
      </ul>
    </div>

    <div class="col d-inline-block">
      <button @click.stop.prevent="getCats()" class="btn w-100">Click to see a cat</button>
      <div v-for="(cat, index) in cats.api" :key="index">
          <img :src="cat.url" alt="" class="cat-image"/>
      </div>
    </div>

  </div>
</template>

<script>
import axios from "axios"

export default {
  data(){
    return{
      cats: {
        api: [],
        newCat: '',
        list: [
            { name: 'Romeo' },
            { name: 'Luiza' },
            { name: 'Henry' }
        ]
      },
      loader: {
        cats: false,
      },
      response: {
          status: "",
          msg: "Ooops! There's was an error, try again later."
      }
    }
  },
  methods: {
    getCats() {
      this.loader.cats = true;

      var url = 'https://api.thecatapi.com/v1/images/search';
      var vm = this;
      axios.get(url).then(function (response) {
          vm.cats.api = response.data;
          vm.loader.cats = false;
      });
    },
    addKitty: function () {
      return this.cats.list.push({ name: this.cats.newCat })
    }
  }
}
</script>

<style>
  .row{
    width: 800px;
    margin-right: auto;
    margin-left: auto;
  }
  .col{
    width: 50%;
    margin: 1rem;
  }
  .d-inline-block{
    display: inline-block;
  }
  .w-100{
    width: 100%;
  }
  input{
    padding: 10px 20px;
    width: -webkit-fill-available;
  }
  .btn{
    padding: 10px 20px;
    background-color: black;
    color: white;
  }
  .cat-image{
    width: 100%;
    height: auto;
  }
</style>
