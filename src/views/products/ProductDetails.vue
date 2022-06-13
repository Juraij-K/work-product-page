<template>
    <div>

    
        <button @click="backward" class="back-btn">Back</button>
        <div v-if="product">
            
            <h1>{{product.title}}</h1>
                <div class="product-img">
                    <img :src="product.images[1]" alt="">
                    <img :src="product.images[2]" alt="">
                    <img :src="product.images[3]" alt="">
                    <img :src="product.images[4]" alt="">
                </div>
                    
                <div class="product-info">
                    <p>Description - {{product.description}}</p>
            <p>Brand {{product.description}}</p> 
            <p>Price {{product.price}}/-</p> 
            <p>Rating {{product.rating}}</p> 
            <p>{{product.stock}} Remaining</p> 
                <button class="Add-Cart-Btn">Add To Cart</button>
                </div>
            
        </div>
        <div v-else>
            <p>Loading job details...</p>
        </div>

  
    </div>
</template>

<script>
export default {
    methods:{
        backward(){
            this.$router.go(-1)
        }
    },

     props: ['id'],
     data(){
        return{
            product:null
        }
    },
    mounted(){
        fetch('http://localhost:3000/products/'+ this.id)
        
        .then(res => res.json())
        .then(data => this.product = data)
        .catch(err => console.log(err.message))
    }
    // data(){
    //     return{
    //         id: this.$route.params.id
    //     }
    // }
}
</script>

<style>
.back-btn{
    width:100px;
    height:50px;
    float: left;
    background: #f3f3f3;
    border-radius: .5em;
    border: none ;
}
.back-btn:hover{
    background: rgb(54, 54, 54);
    color: #fff;
}
.product-img{
    width: 300px;
    height: 100%;
}
.product-img img{
   flex-shrink: 0;
    min-width: 100%;
    min-height: 100%;
    float: left;
}
.Add-Cart-Btn{
    width: 150px;
    height:40px;
    background: #42b983;
    color: #fff;
    font-size: 15px;
    font-weight: 800;
    border: none;
}

</style>