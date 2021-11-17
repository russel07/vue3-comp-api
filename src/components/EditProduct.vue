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
                <el-input v-model="product_title" placeholder="Product Tile"></el-input>
              </el-form-item>
            </el-col>

            <el-col :xs="24" :sm="24" :md="12" :lg="12" :xl="12">
              <el-form-item label="Product Price">
                <el-input v-model="product_price" placeholder="Product Price"></el-input>
              </el-form-item>
            </el-col>
          </el-row>

          <el-row :gutter="20">
            <el-col :span="24">
              <el-input
                  v-model="product_description"
                  :rows="5"
                  type="textarea"
                  placeholder="Product Description"/>
            </el-col>
          </el-row>

          <el-row :gutter="20">
            <el-col :span="6" class="mt-2" :offset="10">
              <el-button type="warning" @click="updateProduct">Update</el-button>

              <el-button type="danger" icon="el-icon-remove" circle @click="toggleForm"></el-button>
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
    name: 'EditProduct',
    props: ["product"],
    emits: ["update-product", "toggle-from"],

    setup(props, context) {console.log(props);
      const product_title = ref(props.product.product_title)
      const product_description = ref(props.product.product_description)
      const product_price = ref(props.product.product_price);

      function updateProduct() {
        let data = {
          product_title: product_title.value,
          product_description: product_description.value,
          product_price: product_price.value
        }
        context.emit("update-product", props.product.id, data);

        product_title.value = '';
        product_description.value = '';
        product_price.value = '';
      }

      function toggleForm(){
        context.emit("toggle-from", true);
      }


      return {
        product_title,
        product_description,
        product_price,
        updateProduct,
        toggleForm
      }
    }
  }
</script>