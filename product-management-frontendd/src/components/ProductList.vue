<template>
  <div class="container mt-5 mb-5">
      <div class="row d-flex">
          <div >
            <div class="d-flex">
              <button @click="openProductForm" type="primary" class="butt">
                <span>  
                  Add Product
                </span></button>
              <h1>Prouct List</h1>
              <div>
                <button id="butto" @click="setFilter('all')" :class="{ 'active': currentFilter === 'all' }">All</button>
                <button id="butto" @click="setFilter('available')" :class="{ 'active': currentFilter === 'available' }">Available</button>
                <button id="butto" @click="setFilter('outOfStock')" :class="{ 'active': currentFilter === 'outOfStock' }">Out of Stock</button>
              </div>
            </div>
              <div v-for="product in filteredProducts" :key="product.id" class="card">
                  <div class="comments-box">
                      <div class="eva">
                        <h3 class="name1"> {{product.name}}</h3>
                        <h3 class="name2" >
                          Available
                          <i v-if="product.available" class="fas fa-check"></i>
                          <i v-if="!product.available" class="fas fa-times"></i>
                        </h3>
                      </div>
                      <h3 class="deliver">Price: {{product.price}}$ </h3>
                      <h3 class="deliver">Quantity: {{product.quantity}} </h3>
                      <h3 >Description: {{product.description}}</h3>
                      <div class="buttons">
                        <button @click="editProduct(product)" class="bdge">Edit</button>
                        <button @click="deleteProduct(product.id)" class="bdge" style="background-color: brown;">Delete</button>
                      </div>
                  </div>
              </div>
              <transition name="modal">
              <product-form
                v-if="showProductForm"
                @close="closeProductForm"
                @save="saveProduct"
                :product="editingProduct"
              />
  </transition>
          </div>
      </div>
  </div>
</template>
  
  <script>
  import ProductForm from './ProductForm.vue';
  import axios from 'axios'
  export default {
    components: {
      ProductForm,
    },
    data() {
      return {
        products: [],
        showProductForm: false,
        currentFilter: 'all',
        editingProduct: {},
      };
    },
    computed: {
    filteredProducts() {
      if (this.currentFilter === 'all') {
        return this.products;
      } else {
        return this.products.filter(product => {
          return this.currentFilter === 'available' ? product.available : !product.available;
        });
      }
    },
  },
    mounted() {
      this.fetchProducts();
    },
    methods: {
      setFilter(filter) {
      this.currentFilter = filter;
    },
      fetchProducts() {
      axios.get('http://localhost:3000/products')
        .then(response => {
          this.products = response.data;
        })
        .catch(error => {
          console.error('Error fetching products:', error);
        });
    },
    openProductForm() {
      this.showProductForm = true;
    },
    closeProductForm() {
      this.showProductForm = false;
    },
    editProduct(product) {
      // Set the product data to the form data
      this.editingProduct = { ...product };
      // Open the ProductForm modal
      this.showProductForm = true;
    },
      deleteProduct(productId) {
      axios.delete(`http://localhost:3000/products/${productId}`)
        .then(() => {
          this.fetchProducts();
        })
        .catch(error => {
          console.error('Error deleting product:', error);
        });
    },
      saveProduct(productData) {
      if (this.editingProduct.id) {
        axios.put(`http://localhost:3000/products/${this.editingProduct.id}`, productData)
          .then(() => {
            this.fetchProducts();
            this.closeProductForm();
            this.editingProduct = {}
          })
          .catch(error => {
            console.error('Error editing product:', error);
          });
      } else {
        axios.post('http://localhost:3000/products', productData)
          .then(() => {
            this.fetchProducts();
            this.closeProductForm();
          })
          .catch(error => {
            console.error('Error adding new product:', error);
          });
      }
    },
    },
  };
  </script>
<style scoped>
.butt {
 align-items: center;
 background-image: linear-gradient(144deg,#AF40FF, #5B42F3 50%,#00DDEB);
 border: 0;
 border-radius: 8px;
 box-shadow: rgba(151, 65, 252, 0.2) 0 15px 30px -5px;
 box-sizing: border-box;
 color: #FFFFFF;
 display: flex;
 font-family: Phantomsans, sans-serif;
 font-size: 18px;
 justify-content: center;
 line-height: 1em;
 max-width: 100%;
 min-width: 140px;
 padding: 3px;
 text-decoration: none;
 user-select: none;
 -webkit-user-select: none;
 touch-action: manipulation;
 white-space: nowrap;
 cursor: pointer;
 transition: all .3s;
}
.modal-enter-active, .modal-leave-active {
  transition: opacity 0.5s;
}

.modal-enter-from, .modal-leave-to {
  opacity: 0;
}
.butt:active,
.butt:hover {
 outline: 0;
}

.butt span {
 background-color: rgb(5, 6, 45);
 padding: 16px 24px;
 border-radius: 6px;
 width: 100%;
 height: 100%;
 transition: 300ms;
}

.butt:hover span {
 background: none;
}

.butt:active {
 transform: scale(0.9);
}
.eva{
  display: flex;
  justify-content:center;
  align-items:center;
}
.comments-box{
    border:2px solid rgb(58, 81, 85);
    margin: 4px;
    border-radius:6px;
  }
  .name1{
    display: inline-block;
    margin-right: calc(30%);
  }
  .name2{
    margin-left: calc(10%);
    margin-right: 2px;
    display: inline-block;
  }
  .deliver{
    display: inline-block;
    margin-right: 5px;
  }
  .fa-check{
    color: chartreuse;
  }
  .fa-times{
    color: rgb(255, 84, 5);
  }
  .bdge{
    background-color: #4974bf;
    color: #ef0505;
    border: none;
    border-radius: 8px;
    transition: .3s;
    font-size: 20px;
    display:flex;
    justify-content:center;
    align-items:center;
    margin-bottom: 10px;
    margin-left:10px;
    display: inline;
    color:#f1ecec;
    cursor: pointer;
  }
.bdge:hover {
  background-color: #3b82f6;
  box-shadow: 0 0 0 5px #3b83f65f;
  color: #fff;
}
.buttons{
  margin-left: calc(60%);
}
.card {
  border-radius: 20px 40px 80px 40px;
  border-left: 5px solid rgb(127, 30, 27);
  transition: 0.2s;
}
.card:hover{
  transform: scale(1.05);
}
.modal-mask {
    position: fixed;
    z-index: 9998;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: table;
    transition: opacity 0.3s ease;
  }

  .modal-wrapper {
    display: table-cell;
    vertical-align: middle;
  }

  .modal-container {
    width: 300px;
    margin: 0px auto;
    padding: 20px 30px;
    background-color: #fff;
    border-radius: 2px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
    transition: all 0.3s ease;
    font-family: Helvetica, Arial, sans-serif;
  }

  .modal-header h3 {
    margin-top: 0;
    color: #42b983;
  }

  .modal-body {
    margin: 20px 0;
  }

  .modal-default-button {
    float: right;
    padding: 6px 12px;
    background-color: #3498db;
    color: #fff;
    border: none;
    cursor: pointer;
  }

  .modal-enter-from, .modal-leave-to {
    opacity: 0;
  }

  .modal-enter-active .modal-container,
  .modal-leave-active .modal-container {
    -webkit-transform: scale(1.1);
    transform: scale(1.1);
  }
  #butto {
  margin: 4px;
  padding: 0.5rem 1rem;
  border-radius: .5rem;
  border: none;
  font-size: 1rem;
  font-weight: 400;
  color: #f4f0ff;
  text-align: center;
  backdrop-filter: blur(10px);
  cursor: pointer;
}

#butto::before {
  content: "";
  display: block;
  position: absolute;
  left: 0;
  top: 0;
  height: 100%;
  width: 100%;
  border-radius: .5rem;
  background: linear-gradient(180deg, rgba(8, 77, 126, 0) 0%, rgba(2, 19, 32, 0.42) 100%),rgba(3, 30, 30, 0.24);
  box-shadow: inset 0 0 12px rgba(151,200,255,.44);
  z-index: -1;
}

#butto::after {
  content: "";
  display: block;
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(180deg, rgba(8, 77, 126, 0) 0%, rgba(8, 77, 126, 0.42) 100%),rgba(47,255,255,.24);
  box-shadow: inset 0 0 12px rgba(151,200,255,.44);
  border-radius: .5rem;
  opacity: 0;
  z-index: -1;
  transition: all .3s ease-in;
}

#butto:hover::after {
  opacity: 1;
}
  </style>