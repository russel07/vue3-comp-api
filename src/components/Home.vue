<template>
  <Menu/>

    <el-form :model="product" class="demo-form-inline">
      <el-row>
        <el-col  :span="16" :offset="4">
          <el-card class="box-card mt-2">
            <template #header>
              <div class="card-header">
                <span>Create / Edit Product</span>
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
                <el-button v-if="!edit" type="primary" @click="createProduct">Create</el-button>
                <el-button v-else type="warning" @click="updateProduct">Update</el-button>
              </el-col>
            </el-row>

          </el-card>
        </el-col>
      </el-row>
    </el-form>

  <el-row>
    <el-col  :span="16" :offset="4">
      <el-card class="box-card mt-2">
        <template #header>
          <div class="card-header">
            <span>User List</span>
          </div>
        </template>
        <el-table
            :data="
            tableData.filter(
              (data) =>
                !search || data.product_title.toLowerCase().includes(search.toLowerCase())
            )
          "
            style="width: 100%"
        >
          <el-table-column label="Title" prop="product_title" />
          <el-table-column label="Description" prop="product_description" />
          <el-table-column label="Price" prop="product_price" />
          <el-table-column align="right">
            <template #header>
              <el-input v-model="search" size="mini" placeholder="Type to search" />
            </template>
            <template #default="scope">
              <el-button size="mini" @click="handleEdit(scope.$index, scope.row)"
              >Edit</el-button
              >
              <el-popconfirm
                  confirm-button-text="OK"
                  cancel-button-text="No, Thanks"
                  icon="el-icon-info"
                  icon-color="red"
                  title="Are you sure to delete this?"
                  @confirm="handleDelete(scope.$index, scope.row)"
              >
                <template #reference>
                  <el-button
                      size="mini"
                      type="danger">Delete</el-button>
                </template>
              </el-popconfirm>

            </template>
          </el-table-column>
        </el-table>
      </el-card>
    </el-col>
  </el-row>
</template>

<script>
import Menu from "./Menu";
export default {
  name: 'Home',
  components: {
    Menu
  },
  data() {
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
      let data = {
        product_title: this.product.product_title,
        product_description: this.product.product_description,
        product_price: this.product.product_price
      }
      this.tableData.push(data);
      this.resetProduct();
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
  },
}
</script>

<style>
  .mt-2{
    margin-top: 2%;
  }
</style>
