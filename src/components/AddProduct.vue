<template>
  <el-form class="demo-form-inline">
    <el-row>
      <el-col  :span="16" :offset="4">
        <el-card class="box-card mt-2">
          <template #header>
            <div class="card-header">
              <span>Create Product</span>
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
              <el-button type="primary" @click="createProduct">Create</el-button>

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
  import Messages from '../composeables/Messages.js';

  export default {
    name: 'AddProduct',
    emits: ["add-product", "toggle-from"],

    setup(_, context) {
      const { success, warning, info, error } = Messages();
      const product_title = ref("");
      const product_description = ref("");
      const product_price = ref("");

      function toggleForm(){
        context.emit("toggle-from", true);
      }

      function createProduct() {
        let data = {
          product_title: product_title.value,
          product_description: product_description.value,
          product_price: product_price.value
        }
        context.emit("add-product", data);
        success("Lets display this");
        warning("This is warning messaeg");
        info("This is info message");
        error("This is error");

        product_title.value = '';
        product_description.value = '';
        product_price.value = '';
      }

      return {
        product_title,
        product_description,
        product_price,
        createProduct,
        toggleForm
      }
    }
  }
</script>