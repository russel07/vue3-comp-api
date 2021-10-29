<template>
  <Menu/>
  <add-product v-if="!edit" @add-product="createProduct"></add-product>
  <edit-product v-else :product="existingProduct" @update-product="updateProduct"></edit-product>
  <products :products="filteredTableData" @search="handleSearch" @delete-product="handleDelete" @edit-product="handleEdit"></products>
</template>

<script>
import Menu from "./Menu";
import AddProduct from "./AddProduct";
import Products from "./Products";
import EditProduct from "./EditProduct";

import { ref, computed } from 'vue';

export default {
  name: 'Home',
  components: {
    Menu,
    AddProduct,
    Products,
    EditProduct
  },
  setup(){
    const edit = ref(false);
    const search = ref(false)
    const tableData = ref([]);
    const existingProduct = ref({});

    tableData.value.push({
      product_title: 'Test Title',
      product_description: 'Test description',
      product_price: '$120'
    })

    const filteredTableData = computed(function(){
      return tableData.value.filter(
          (data) => !search.value || data.product_title.toLowerCase().includes(search.value.toLowerCase())
      )
    })

    const createProduct = function(data){
      tableData.value.push(data);
    }

    const handleEdit = function(data){
      edit.value = true;
      existingProduct.value = data
    }

    const updateProduct = function(id, row){
      tableData.value.splice(id,1,row);
      edit.value = false
    }

    const handleDelete = function(index) {
      tableData.value.splice(index,1);
    }

    const handleSearch = function(query){
      search.value = query;
    }

    return {
      filteredTableData,
      createProduct,
      edit,
      existingProduct,
      handleEdit,
      updateProduct,
      handleDelete,
      handleSearch
    }
  },
  /*data() {
    return {
      product: {
        product_title: '',
        product_description: '',
        product_price: '',
        id: ''
      },
      tableData: [],
      search: '',
      edit: false
    }
  },
  methods: {
    createProduct() {

    },
    updateProduct(){
      let data = {
        product_title: this.product.product_title,
        product_description: this.product.product_description,
        product_price: this.product.product_price
      }
      this.tableData.splice(this.product.id,1,data);
      this.edit = false
      this.resetProduct();
    },
    handleEdit(index, row) {
      this.product.product_title = row.product_title;
      this.product.product_description = row.product_description;
      this.product.product_price = row.product_price;
      this.product.id = index;
      this.edit = true;
    },
    handleDelete(index, row) {
      console.log(index, row)
      this.tableData.splice(this.product.id,1);
    },
    resetProduct(){
      this.product.product_title = '';
      this.product.product_description = '';
      this.product.product_price = '';
    }
  },*/
}
</script>

<style>
  .mt-2{
    margin-top: 2%;
  }
</style>
