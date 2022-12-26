<template>
  <div class="container">
    <div>
      <button class="btn btn-primary btn-lg" @click="changeView">change view</button>
      <a class="btn btn-primary btn-lg btn-size" @click="changeSort">sort
        <img class="svg" v-if="sortPrice == 1" src="./assets/icons/arrow-up-bold-circle.svg" />
        <img class="svg" v-if="sortPrice == -1" src="./assets/icons/arrow-down-bold-circle.svg" />
      </a>
      <button class="btn btn-primary btn-lg btn-size" @click="change">filter</button>
    </div>
    <br />

    <div class="row">
      <div v-if="detailView" class="col-12 col-sm-6 col-md-6 col-lg-4 col-xl-3" v-for="item in list">
        <div class="detailView">
          <img v-bind:src="item.image" />
          <div class="details">
            <span v-html="item.price + 'Euro'"></span><br />
            <span v-html="item.shortDescription"></span><br />
            <span v-html="item.name"></span>
          </div>
        </div>
      </div>
      <div v-else class="col-12 col-sm-6 col-md-4 col-lg-3 col-xl-2" v-for="item in list">
        <div class="bottleView">
          <img v-bind:src="item.image" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      list: [],
      sortPrice: 0,
      detailView: true,
      all: [],
    }
  },
  methods: {
    changeView: function () {
      this.list = this.all
      return this.detailView ? this.detailView = false : this.detailView = true;
    },
    changeSort: function () {
      if (this.sortPrice == -1 || this.sortPrice == 0) {
        this.list = this.all.sort((a, b) => a.price - b.price)
        this.sortPrice = 1
      }
      else {
        this.list = this.all.sort((a, b) => b.price - a.price)
        this.sortPrice = -1
      }
    },
    change: function () {
      this.list = this.all.filter(element => element.pricePerUnitText >= "(2 €/Liter)");
    }
  },
  mounted: function () {
    fetch("https://flapotest.blob.core.windows.net/test/ProductData.json")
      .then(response => response.json())
      .then(data => {
        data.forEach(element => {
          element.articles.forEach(item => {
            this.all.push({ name: element.name, image: item.image, price: item.price, shortDescription: item.shortDescription, pricePerUnitText: item.pricePerUnitText })
          });
        });
        this.list = this.all
      });
  }
}
</script>

<style scoped>
.detailView {
  display: flex;
  width: 100%;
  padding: 10px;
  border: 2px solid black;
  border-radius: 5px;
  margin: 10px;
  background-color: lightgrey;
}

.bottleView {
  display: flex;
  width: 100%;
  border: 2px solid black;
  border-radius: 5px;
  margin: 10px;
  justify-content: center;
  background-color: lightgrey;
}

.bottleView img,
.detailView img {
  width: 30px;
  height: 100px;
  margin: 10px;
}

.details {
  text-align: right;
  width: 100%;
}

.btn {
  margin: 10px;
  background-color: gray;
  color: black;
  border-color: gray;
}

.svg {
  width: 24px;
}

button:hover,
a:hover {
  background-color: #c1c1c1;
  color: black;
  border-color: black;
}

.btn-size {
  width: 100px;
}
</style>
