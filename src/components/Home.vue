<template>
  <div class="home"><br>
    
    <div class="product" style="margin-top:100px;">
      <h3 class="container">{{ title }}</h3>
        <ul class="media-list">
            <li class="media mb-4" v-for="img in product" v-bind:src="img">
              <div class="media-left mr-4">
                <img class="media-object" v-bind:src="img"/>
              </div>
              <div class="media-body">
                <h4 class="media-heading">
                  <a v-bind:href="url" target="_blank">{{name}}</a>
                </h4><br>
                <p><span style="float:left;"><b>{{brand}}</b></span><br><span style="float:right; margin-left:50px; font-size:2em;"><b>{{price}}</b></span></p>
                <p v-if="inventory > 10">In stock</p>
                <p v-else-if="inventory <= 10 && inventory > 0"  style="color:red;">Low stock!</p>
                <p v-else>Out of stock</p>
                <p v-if="onSale">On Sale!</p>
                <ul>
                  <li v-for="detail in details">{{detail}}</li>
                </ul>
                <div v-for="variant in variants" :key="variant.variantId">
                  <p>{{variant.variantColor}}</p>
                </div>
                <p><select class="form-control" style="width:100px;">
                  <option v-for="size in sizes">{{size}}</option>
                </select></p>
                <button v-on:click="cart +=1" v-if="inventory">Add to Cart</button><button style="width:180px; float: right;"><i class="fas fa-shopping-cart"></i>&nbsp;<span>Cart({{cart}})</span></button>
                
              </div>
            </li>
            
        </ul>
        <div class="product-info">
            
        </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Home',
  data () {
    return {
      name: 'Jacket',
      title: 'Featured Product',
      product: [require('@/assets/pictures/brown_jacket.png'),
      require('@/assets/pictures/black_jacket.png'),
      require('@/assets/pictures/brown_jacket.png'),
      require('@/assets/pictures/pink_jacket.png')],
      brand: 'next',
      url: 'leighton.com/product_id',
      price: '£65',
      inventory: 5,
      onSale: true,
      details: ["40% polyester", "20% cotton", "30% linen", "10% silk"],
      variants: [
        {
          variantId: 2234,
          variantColor: "green"
        },
        {
          variantId: 2235,
          variantColor: "red"
        },
        {
          variantId:2236,
          variantColor: "blue"
        }
      ],
      sizes: ["small", "medium", "large"],
      cart: 0,
      sources:[],
      source: ''

    }
  },
  created() {
    this.fetchSources();
  },
  methods: {
    fetchSources() {
      this.$http.get('https://27gmrimn45.execute-api.eu-west-2.amazonaws.com/demos/leighton-demo-api?TableName=products', {
        headers: {
          "Access-Control-Allow-Origin": "*",
          "x-api-key": "zQo4PPqD862IwDIQRZub8gX4dqjA3aW2DDhI6UF4"
        }
      })
        .then(response => {
            this.sources = response.data.sources;
            console.log(response.data);
        });
    }
  } 
       
}
</script>

<style scoped>
.product-image {
    margin: 30px;
    padding: 30px;
}
</style>