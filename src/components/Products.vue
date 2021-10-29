<template>
  <el-row>
    <el-col  :span="16" :offset="4">
      <el-card class="box-card mt-2">
        <template #header>
          <div class="card-header">
            <span>Product List</span>
          </div>
        </template>
        <el-table
            :data="products"
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
                  @confirm="handleDelete(scope.$index)"
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
  import { ref, watch } from 'vue';

  export default {
    name: 'Products',
    props: ["products"],
    emits: ["delete-product", "search"],

    setup(_, context) {
      const search = ref('');

      function handleEdit(index, row){
        let data = {
          product_title: row.product_title,
          product_description: row.product_description,
          product_price: row.product_price,
          id: index
        }
        context.emit("edit-product", data);
      }

      function handleDelete(index){
        context.emit("delete-product", index);
      }

      watch(search, function (val) {
        if(val){
          context.emit("search", val);
        }
      });

      return {
        search,
        handleEdit,
        handleDelete
      }
    }
  }
</script>