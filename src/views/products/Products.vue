<template>
    <div class="product-list">
        <h1>Products</h1>
        
        
        <input id="search-box" type="text" v-model="search" placeholder="search for products">
                <h4>Filter</h4>
                <!-- Filter by categories -->
                <select class="productFilter" v-model="filterByCategory">
                    <option value="">All Categories</option>
                    <option :value="product" v-for="product in eliminateDuplicatedCategories(products)" :key="product.id">
                        {{product}}
                    </option>
                </select>
                <!-- Filter by brands -->
                <select class="productFilter" v-model="filterByBrand">
                    <option value="">All Brands</option>
                    <option :value="product" v-for="product in eliminateDuplicatedBrands(products)" :key="product.id">
                        {{product}}
                    </option>
                </select>
                <h4>Sort</h4>
                <!-- Sort by prices -->
                <select class="form-select" v-model="sortByPrice" @change="sortProductsByPrice(products)">
                    <option value="" disabled>Sort By Price</option>
                    <option value="increasingOrder">Increasing Order</option>
                    <option value="decreasingOrder">Decreasing Order</option>
                </select>
                <!-- Sort by ratings -->
                <select class="form-select " v-model="sortByRating" @change="sortProductsByRating(products)">
                    <option value="" disabled>Sort By Rating</option>
                    <option value="increasingOrder">Increasing Order</option>
                    <option value="decreasingOrder">Decreasing Order</option>
                </select>
                <!-- Sort by discounts -->
                <select class="form-select " v-model="sortByDiscount" @change="sortProductsByDiscount(products)">
                    <option value="" disabled>Sort By Discounts</option>
                    <option value="increasingOrder">Increasing Order</option>
                    <option value="decreasingOrder">Decreasing Order</option>
                </select>

        
        <div v-if="products.length">
            
                <div v-for="product in filteredProducts" :key="product.id"   class="product">
                    <router-link :to="{ name: 'ProductDetails', params:{ id: product.id }}">
                        <div class="product-thumbnail">
                            
                                
                            <img :src="product.thumbnail" alt="">
                            <h2 class="product-title">{{product.title}}</h2>
                            <h2 class="product-price">USD {{product.price}}/-</h2>
                            <h2 class="product-rating">{{product.rating}}</h2>
                        </div>
                    </router-link>
                </div>
            
            
        </div>
        <div v-else>
            <p>Loading products...</p>
        </div>
    
    </div>
</template>

<script>

export default {
    
    data(){
        return{
            products:[],
            search: '',
            filterByCategory: '',
            filterByBrand:'',
            sortByPrice: '',
            sortByRating: '',
            sortByDiscount: ''
        }
    },
    
    mounted(){
        fetch('http://localhost:3000/products')
        
        .then(res => res.json())
        
        .then(data => this.products = data)
        .catch(err => console.log(err.message))
    },
    computed:{
        filteredProducts: function(){
            return this.products.filter((product) =>{
                return product.title.toLowerCase().match(this.search.toLowerCase())
            });
        }
    },
    methods:{
        eliminateDuplicatedCategories(products){
            let arr = []
            products.forEach(product => {
                arr.push(product.category)
            })
            return [...new Set(arr)];
        },
        eliminateDuplicatedBrands(products){
            let arr = []
            products.forEach(product => {
                arr.push(product.brand)
            })
            return [...new Set(arr)];
        },
        capitalized(data){
            return data.charAt(0).toUpperCase() + data.slice(1)
        },
        sortProductsByPrice(products){
            this.sortByRating = ''
            if(this.sortByPrice == 'increasingOrder'){
                return products.sort((a, b) => a.price - b.price)
            }else if(this.sortByPrice == 'decreasingOrder'){
                return products.sort((a, b) => b.price - a.price)
            }
        },
        sortProductsByRating(products){
            this.sortByPrice = ''
            if(this.sortByRating == 'increasingOrder'){
                return products.sort((a, b) => a.rating - b.rating)
            }else if(this.sortByRating == 'decreasingOrder'){
                return products.sort((a, b) => b.rating- a.rating)
            }
        },
        sortProductsByDiscount(products){
            this.sortByPrice = ''
            if(this.sortByDiscount == 'increasingOrder'){
                return products.sort((a, b) => a.discountPercentage - b.discountPercentage)
            }else if(this.sortByDiscount == 'decreasingOrder'){
                return products.sort((a, b) => b.discountPercentage- a.discountPercentage)
            }
        }
    }
    
}


</script>

<style>

.product-list{
  
}
.product{
    display: flex;
    margin: 0 auto;
    float: left;
}
.product .product-thumbnail{
    width: 300px;
    height: 350px;
    background: #f4f4f2;
    color: #fff;
    text-shadow: 3px 3px #000;
    

    display: flex;
    justify-content: center;
    align-items: center;
    overflow: hidden;
    position: relative;

    border-radius: 10%;
    cursor: pointer;
    float: left;
    margin: 1em;
}
.product .product-thumbnail:hover{
    background: #ddd;
}
.product .product-thumbnail img{
    flex-shrink: 0;
    min-width: 100%;
    min-height: 100%
}
.product-title, .product-price, .product-rating{
    position: absolute;
    left: 30px;
}
.product-title{
    bottom: 45px;
}
.product-price{
    bottom: 15px;
}
.product-rating{
    top: 15px;
}
.product a{
    text-decoration: none;
}
nav button{
    width: auto;
    height: auto;
    
    border:none;
}
nav button:hover{
 background: #42b983;
}
#search-box{
   width: 300px; 
   height: 30px;
   
}
.productFilter, .form-select{
    width: 305px;
    height: 35px;
    margin: 5px;
}
</style>