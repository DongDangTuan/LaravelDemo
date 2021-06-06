<template>
    <div>
        <h3 class="text-center">Edit product</h3>
        <div class="row">
            <div class="col-md-6">
                <div class="form-group">
                    <label class="label-control col-md-4">Name</label>
                    <div class="col-md-8">
                        <input type="text" class="form-control" v-model="product.name">
                    </div>
                </div>
                     <div class="form-group">
                    <label class="label-control col-md-4">Detail</label>
                    <div class="col-md-8">
                        <input type="text" class="form-control" v-model="product.detail">                  
                    </div>
                   
                </div>
                <div class="col-md-12">
                 <button type="button" @click="updateProduct()" class="btn btn-primary">Edit</button>
                </div>

            </div>
        </div>
    </div>
</template>
<script>
export default {
    data(){
        return {
            product:{}
        }
    },
    created(){
        this.axios
            .get('http://localhost:8000/api/products/'+this.$route.params.id)
            .then((response)=>{
                this.product=response.data;
            });
    },
    methods:{
        updateProduct(){
            this.axios
                .patch('http://localhost:8000/api/products/'+this.$route.params.id,this.product)
                .then(response=>{
                    this.$router.push({name:'home'})
                })
                .catch(err=>console.log(err))
                .finally(()=>this.loading=false)
        }
    }
}
</script>