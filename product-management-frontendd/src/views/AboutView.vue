<template>
  <div class="about-page">
    <h2>About Product Management System</h2>
    <p>
      Welcome to the Product Management System, a web application designed to help users manage their product inventory efficiently.
    </p>

    <h3>Key Features</h3>
    <ul>
      <li>Product Creation: Easily add new products with details like name, description, price, and quantity in stock.</li>
      <li>Product List: View and manage your product inventory in a list format with filtering options.</li>
      <li>Product Updates: Edit existing product details and mark products as available or out of stock.</li>
      <li>Product Deletion: Delete products when needed, and keep your inventory organized.</li>
    </ul>

    <h3>Technology Stack</h3>
    <ul>
      <li>Frontend: Vue.js</li>
      <li>Backend API: NestJS</li>
      <li>Database: PostgreSQL with Prisma ORM</li>
    </ul>

    <h3>Developer</h3>
    <ul>
      <li>Abrham Belayineh - Frontend Developer</li>
      <li>Abrham Belayineh - Backend Developer</li>
    </ul>

    <p>
      For any inquiries or support, please contact me at abrsh6266@gmail.com.
    </p>
  </div>
</template>

<script>
export default {
  name: 'AboutPage',
};
</script>

<style scoped>
.about-page {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

h2, h3 {
  color: #3498db;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  margin-bottom: 8px;
}

p {
  line-height: 1.6;
}
</style>
