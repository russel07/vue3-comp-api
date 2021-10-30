<template>
  <Menu/>
  <add-product v-if="addForm" @add-product="createProduct" @toggle-from="toggleForm"></add-product>
  <edit-product v-if="editForm" :product="existingProduct" @update-product="updateProduct"  @toggle-from="toggleForm"></edit-product>
  <products :products="filteredTableData" @add-form="TriggerAddForm" @search="handleSearch" @delete-product="handleDelete" @edit-product="handleEdit"></products>
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
    const addForm = ref(false);
    const editForm = ref(false);
    const search = ref(false)
    const tableData = ref([]);
    const existingProduct = ref({});

    function TriggerAddForm(){
      editForm.value = false;
      addForm.value = true;
    }

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
      addForm.value = false;
    }

    const handleEdit = function(data){
      addForm.value = false;
      editForm.value = true;
      existingProduct.value = data
    }

    const updateProduct = function(id, row){console.log(row);
      tableData.value.splice(id,1,row);
      editForm.value = false
    }

    const handleDelete = function(index) {
      tableData.value.splice(index,1);
    }

    const handleSearch = function(query){
      search.value = query;
    }

    function toggleForm(){
      editForm.value = false;
      addForm.value = false;
    }

    return {
      addForm,
      TriggerAddForm,
      filteredTableData,
      createProduct,
      editForm,
      existingProduct,
      handleEdit,
      updateProduct,
      handleDelete,
      handleSearch,
      toggleForm
    }
  }
}
</script>

<style>
  .mt-2{
    margin-top: 2%;
  }

  .text-right {
    float: right;
  }
</style>
