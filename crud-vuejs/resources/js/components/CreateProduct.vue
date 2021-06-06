<template>
   
  <div>
    <h3 class="text-center">Create product</h3>
    <div class="row">
      <div class="col-md-6">
        <div class="form-group">
          <label class="label-control col-md-4">Name</label>
          <div class="col-md-8">
            <input type="text" class="form-control" v-model="product.name" />
          </div>
        </div>
        <div class="form-group">
          <label class="label-control col-md-4">Detail</label>
          <div class="col-md-8">
            <input type="text" class="form-control" v-model="product.detail" />
          </div>
        </div>
        <div class="form-group">
                <input type="file" @change="onFileChange" class="form-control"/>          
        </div>
      
        <div class="col-md-12">
          <button type="button" @click="addProduct()" class="btn btn-primary">
            Create
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
  import EditProduct from './EditProduct.vue';
export default {
  
  data() {
    return {
      product: {},
      SelectedFile: null,
    };
  },
  methods: {
    onFileChange(e) {
      this.SelectedFile = e.target.files[0];
      console.log(this.SelectedFile);
    },
    addProduct() {
      //    headers: {
      // Authorization: "Bearer "+localStorage.getItem('token')
      // }
      const config = {
        headers: {
          "content-type": "multipart/form-data",
        },
      };

      let form = new FormData();
      form.append("file", this.SelectedFile);
      this.axios
        .post("http://localhost:8000/api/upload", form, config)
        .then((res) => {
          this.product.image = res.data;
          console.log(this.product.image);
          this.axios
            .post("http://localhost:8000/api/products/", this.product)
            .then((response) => {
              this.$router.push({ name: "home" });
            })
            .catch((err) => console.log(err))
            .finally(() => (this.loading = false));
        });
    },
  },
};
</script>