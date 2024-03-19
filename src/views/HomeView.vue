<template>
    <div class="row">
      <div class="col-lg-4 offset-lg-2 mb-3">
        <input type="text" v-model="categoryFilter" @input="searchProducts" class="form-control" placeholder="Buscar por una  categoría de : Electrónica, Ropa, Libros o Deportes">
      </div>
      <div class="col-lg-4 mb-3">
        <button @click="goToCreateProductView" class="btn btn-success">
          <i class="fa-solid fa-plus"></i> Crear Producto
        </button>
      </div>
    </div>
    <div class="row">
      <div class="col-lg-8 offset-lg-2 col-sm-12">
        <div class="table-responsive">
          <table class="table table-bordered table-hover border-dark">
            <thead class=" bg-success">
              <tr>
                <th>#</th>
                <th>NOMBRE</th>
                <th>DESCRIPCION</th>
                <th>PRECIO</th>
                <th colspan="2">ACCIONES</th>
              </tr>
            </thead>
            <tbody class="table-group-divider " id="contenido">
              <tr class="bg-dark bg-opacity-10" v-for="product in products" :key="product.id">
                <td>{{ product.id }}</td>
                <td>{{ product.name }}</td>
                <td>{{ product.description }}</td>
                <td>${{ product.price }}</td>
                <td>
                  <button @click="editProduct(product.id)" class="btn btn-sm btn-warning mr-3">
                    <i class="fa-solid fa-edit"></i> 
                  </button>
                </td>
                <td>
                  <button @click="deleteProduct(product.id)" class="btn btn-sm btn-danger mr-3">
                    <i class="fa-solid fa-trash"></i> 
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import Swal from 'sweetalert2';
  import axios from 'axios';
  
  export default {
    data() {
      return {
        products: [],
        categoryFilter: '', // Nueva propiedad para la búsqueda por categoría
      };
    },
    mounted() {
      this.getProducts();
    },
    methods: {
      async getProducts() {
        try {
          let url = 'https://apiproduct-nveo.onrender.com/products';
          // Agregar el parámetro de categoría si categoryFilter no está vacío
          if (this.categoryFilter.trim() !== '') {
            url += `/category/${this.categoryFilter.trim()}`;
          }
          const response = await axios.get(url);
          this.products = response.data;
        } catch (error) {
          console.error('Error al obtener los productos:', error);
        }
      },
      // Método para buscar productos por categoría
      searchProducts() {
        this.getProducts();
      },
      async deleteProduct(productId) {
        // Mostrar un mensaje de confirmación con SweetAlert2
        const result = await Swal.fire({
          title: '¿Estás seguro?',
          text: 'No podrás revertir esta acción',
          icon: 'warning',
          showCancelButton: true,
          confirmButtonColor: '#3085d6',
          cancelButtonColor: '#d33',
          confirmButtonText: 'Sí, eliminar'
        });
  
        // Si el usuario confirma la eliminación
        if (result.isConfirmed) {
          try {
            const response = await axios.delete(`https://apiproduct-nveo.onrender.com/products/${productId}`);
            console.log('Producto eliminado:', response.data);
            // Mostrar alerta de éxito
            await Swal.fire(
              '¡Eliminado!',
              'El producto ha sido eliminado correctamente.',
              'success'
            );
            // Actualizar la lista de productos después de eliminar uno
            this.getProducts();
          } catch (error) {
            console.error('Error al eliminar el producto:', error);
            // Mostrar alerta de error
            await Swal.fire(
              'Error',
              'No se pudo eliminar el producto.',
              'error'
            );
          }
        }
      },
      editProduct(productId) {
        // Redirige a la vista de edición del producto con el ID productId
        this.$router.push({ name: 'EditProduct', params: { id: productId } });
        console.log('Editando producto con ID:', productId);
      },
      goToCreateProductView() {
        // Redirige a la vista de creación de producto
        this.$router.push({ name: 'create-product' });
      }
    }
  };
  </script>
  
  <style>
  /* Estilos CSS */
  </style>
  