<template>
  <div class="modal-mask">
    <div class="modal-wrapper">
      <div class="modal-container">
        <div class="modal-header">
          <h3>{{ editing ? 'Edit Product' : 'Add Product' }}</h3>
          <button class="modal-default-button" @click="closeForm">Close</button>
        </div>
        
        <!-- Product Form -->
        <form @submit.prevent="saveProduct">
          <label for="productName">Product Name:</label>
          <input v-model="formData.name" type="text" id="productName" required />

          <label for="productDescription">Description:</label>
          <textarea v-model="formData.description" id="productDescription" required></textarea>

          <label for="productPrice">Price:</label>
          <input v-model.number="formData.price" type="number" id="productPrice" required />

          <label for="productQuantity">Quantity:</label>
          <input v-model.number="formData.quantity" type="number" id="productQuantity" required />

          <label for="productAvailability">Availability:</label>
          <input v-model="formData.available" type="checkbox" id="productAvailability" />

          <button type="submit">{{ editing ? 'Save Changes' : 'Add Product' }}</button>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    editing: {
      type: Boolean,
      default: false,
    },
    product: {
      type: Object,
      default: () => ({}),
    },
  },
  data() {
    return {
      formData: {
        name: this.product.name || '',
        description: this.product.description || '',
        price: this.product.price || 0,
        available: this.product.available || false,
        quantity:this.product.quantity|| 0
      },
    };
  },
  methods: {
    closeForm() {
      this.formData = {
        name: '',
        description: '',
        price: 0,
        available: false,
        quantity:0
      };
      this.$emit('close');
    },
    saveProduct() {
      this.$emit('save', this.formData);
      this.closeForm();
    },
  },
};
</script>

<style scoped>
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
    width: 400px;
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

  label {
    display: block;
    margin-bottom: 8px;
  }

  input,
  textarea {
    width: 100%;
    padding: 8px;
    margin-bottom: 12px;
    box-sizing: border-box;
  }

  button {
    padding: 8px 16px;
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
</style>
