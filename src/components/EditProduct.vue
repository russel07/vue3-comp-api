<template>
  <el-form :model="product" class="demo-form-inline">
    <el-row>
      <el-col  :span="16" :offset="4">
        <el-card class="box-card mt-2">
          <template #header>
            <div class="card-header">
              <span>Edit Product</span>
            </div>
          </template>

          <el-row :gutter="20">
            <el-col :xs="24" :sm="24" :md="12" :lg="12" :xl="12">
              <el-form-item label="Product Tile">
                <el-input v-model="product.product_title" placeholder="Product Tile"></el-input>
              </el-form-item>
            </el-col>

            <el-col :xs="24" :sm="24" :md="12" :lg="12" :xl="12">
              <el-form-item label="Product Price">
                <el-input v-model="product.product_price" placeholder="Product Price"></el-input>
              </el-form-item>
            </el-col>
          </el-row>

          <el-row :gutter="20">
            <el-col :span="24">
              <el-input
                  v-model="product.product_description"
                  :rows="5"
                  type="textarea"
                  placeholder="Product Description"/>
            </el-col>
          </el-row>

          <el-row :gutter="20">
            <el-col :span="6" class="mt-2" :offset="10">
              <el-button type="warning" @click="updateProduct">Update</el-button>
            </el-col>
          </el-row>

        </el-card>
      </el-col>
    </el-row>
  </el-form>
</template>

<script>

  import { ref } from 'vue';

  export default {
    name: 'AddProduct',
    props: ["product"],
    emits: ["update-product"],

    setup(props, context) {
      const product = ref(props.product)

      function updateProduct() {
        let data = {
          product_title: product.value.product_title,
          product_description: product.value.product_description,
          product_price: product.value.product_price
        }
        context.emit("update-product", product.value.id, data);

        product.value.product_title = '';
        product.value.product_title = '';
        product.value.product_title = '';
      }

      return {
        updateProduct
      }
    }
  }
</script>