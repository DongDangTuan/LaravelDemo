<template>
    <div class="container">
        <h1>Laravel CRUD with VUEJS</h1>
        <div class="row">
            <router-link to="/" class="item1"> <span class="glyphicon glyphicon-th-list"></span> Products List</router-link>
            <router-link to="/create" class="item1"> <span class="glyphicon glyphicon-plus"></span> Create Product</router-link>
        </div>
        <router-view></router-view>
    </div>
</template>
<script>
export default {
    
}
</script>
<style scoped>
.item1{
    padding: 10px 20px;
    text-decoration: none;
    color: black;
}
</style>