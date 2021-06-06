<template>
  <div>
    <div class="row">
      <div class="col-md-4">
        <h1 class="text-center">List product</h1>
      </div>
      <div class="col-md-4">
        <input
          type="text"
          class="form-control"
          placeholder="Nhập tên sản phẩm..."
          v-model="keySearch"
        />
      </div>
    </div>
    <table class="table table-bordered">
      <tr>
        <td>STT</td>
        <td>Image</td>
        <td>ID</td>
        <td>Name</td>
        <td>Detail</td>
      </tr>
      <tr v-for="(product, index) in fillProduct" :key="product.id">
        <td>{{ index + 1 }}</td>
        <td>
          <img
            :src="product.image"
            :alt="product.name"
            width="50px"
            height="50px"
          />
        </td>
        <td>{{ product.id }}</td>
        <td>{{ product.name }}</td>
        <td>{{ product.detail }}</td>
        <td>
          <router-link
            :to="{ name: 'edit', params: { id: product.id } }"
            class="btn btn-success"
            >Edit</router-link
          >
          <button class="btn btn-danger" @click="deleteProduct(product.id)">
            Delete
          </button>
        </td>
      </tr>
    </table>
    <nav>
      <ul class="pagination">
        <li v-if="pagination.current_page > 1">
          <a
            href="#"
            v-on:click.prevent="changePage(pagination.current_page - 1)"
            >back</a
          >
        </li>
        <li
          v-for="page in pagesNumber"
          :class="{ 'active': page == pagination.current_page }"
          :key="page"
        >
          <a href="#" v-on:click.prevent="changePage(page)">{{ page }}</a>
        </li>
        <li v-if="pagination.current_page < pagination.last_page">
          <a
            v-on:click.prevent="changePage(pagination.current_page + 1)"
            href="#"
            >Next</a
          >
        </li>
      </ul>
    </nav>
  </div>
</template>

<script>
export default {
  data() {
    return {
      products: [],
      keySearch: "",
      pagination: {
        total: 0,
        from: 1,
        to: 0,
        current_page: 1,
      },
    };
  },
  created() {
    this.axios.get("http://localhost:8000/api/products/").then((response) => {
      this.products = response.data.data;
      this.pagination = response.data;
    });
  },
  methods: {  
    changePage(page){
      this.axios
        .get("http://localhost:8000/api/products?page=" + page)
        .then((res) => {
          this.products = res.data.data;
          this.pagination = res.data;
        });
    },

    deleteProduct(id) {
      this.axios
        .delete("http://localhost:8000/api/products/" + id)
        .then((response) => {
          this.products.forEach((element, index) => {
            if (element.id == id) this.products.splice(index, 1);
          });
          alert(response.data);
        });
    },
  },
  computed: {
    fillProduct: function () {
      return this.products.filter((element) => {
        return element.name.includes(this.keySearch) === true;
      });
    },
      pagesNumber: function () {
    if (!this.pagination.to) return [];
    let from = 1;
    let to = this.pagination.last_page;
    let pagesArray = [];
    for (from = 1; from <= to; from++) {
      pagesArray.push(from);
    }
    return pagesArray;
  },
  }

};
</script>
<style scoped>
  li{
    display:inline-block;
    margin-left:10px;
  }
  .active a{
    color:black
  }
</style>