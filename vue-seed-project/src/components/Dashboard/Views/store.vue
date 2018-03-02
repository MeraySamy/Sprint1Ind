<template>
    <div class="row">
      <div class="col-md-12">
        <div class="card">

          <table border="1"  style="width:100%">
          <th v-for="header in tableColumns">
          {{header}}
          </th>
          <tr v-for="product in tableData">

          <td>{{product.name}}</td>
          <td>{{product.price}}</td>
          <td>{{product.createdAt}}</td>
          <td>{{product.updatedAt}}</td>
          <td>{{product.Seller}}</td>
          <button v-on:click="Delete(product._id)">Delete</button>
          <button v-on:click="Edit(product._id, product.name, product.price, product.Seller )">Edit</button>
          </tr>
          </table>

        </div>

        Update ID:<br>
        <input type="text" v-model="idupdate"><br>
      Name:<br>
      <input type="text" v-model="name"><br>
      Price:<br>
      <input type="text" v-model="price"><br>
      Seller:<br>
      <input type="text" v-model="Seller"><br>
      <button v-on:click="Add()">Add</button>
      <button v-on:click="Edit2()">Edit</button>
      </div>
    </div>

</template>
<script>
  import PaperTable from 'components/UIComponents/PaperTable.vue'
  import axios from 'axios'

  export default {
    components: {
      PaperTable
    },
    data () {
      return {
        tableData: [],
        tableColumns: ['Name ', 'Price ', 'CreatedAt ', 'UpdatedAt ', 'SellerName', 'Actions'],
        name: '',
        price: '',
        Seller: '',
        idupdate: ''
      }
    },
    methods:
    {
      Delete: function (productid) {
        axios.delete('http://localhost:3000/api/product/deleteProduct/' + productid)
      },
      Add: function () {
        axios.post('http://localhost:3000/api/product/createProduct', {
          name: this.name,
          price: this.price,
          Seller: this.Seller
        })
      },
      Edit: function (id, name, price, Seller) {
        this.idupdate = id
        this.name = name
        this.price = price
        this.Seller = Seller
      },
      Edit2: function () {
        axios.patch('http://localhost:3000/api/product/updateProduct/' + this.idupdate, {
          name: this.name,
          price: this.price,
          Seller: this.Seller
        })
      }
    },
    mounted () {
      axios.get('http://localhost:3000/api/product/getProducts')
      .then(response => {
      // JSON responses are automatically parsed.
        this.tableData = response.data.data
      })
    }
  }
</script>
<style>
</style>
