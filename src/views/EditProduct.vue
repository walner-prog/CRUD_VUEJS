<template>
    <div class="container">
        
      
        <div class="row mt-3">
            <div class="col-md-6 offset-md-3">
                <div class="card">
                    <div class="card-header bg-dark text-white text-center">Editar Producto</div>
                    <div class="card-body">
                        <h1>Editar Producto</h1>
                <form @submit.prevent="updateProduct">
                <div class="form-group  mb-3">
                  <label for="name">Nombre:</label>
                  <input type="text" id="name" v-model="editedProduct.name" class="form-control" required>
                </div>
                <div class="form-group  mb-3">
                  <label for="description">Descripción:</label>
                  <textarea id="description" v-model="editedProduct.description" class="form-control" rows="3" required></textarea>
                </div>
                <div class="form-group  mb-3">
                  <label for="price">Precio:</label>
                  <input type="decimal" id="price" v-model="editedProduct.price" class="form-control" required>
                </div>
                <br>
                <button type="submit" class="btn btn-primary">Guardar Cambios</button>
                <div class="row mt-3">
                    <div class="col-md-6 offset-md-3">
                      <router-link to="/home" class="btn btn-secondary mb-3">
                        Volver a la lista de productos
                      </router-link>
                    </div>
                  </div>
              </form>
                    </div>
                </div>
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
        editedProduct: {
          id: null,
          name: '',
          description: '',
          price: null
        }
      };
    },
    mounted() {
      // Aquí podrías cargar los datos del producto a editar utilizando su ID
      const productId = this.$route.params.id;
      this.loadProduct(productId);
    },
    methods: {
      async loadProduct(productId) {
        try {
          const response = await axios.get(`https://apiproduct-nveo.onrender.com/products/${productId}`);
          this.editedProduct = response.data;
        } catch (error) {
          console.error('Error al obtener el producto:', error);
        }
      },
      async updateProduct() {
        try {
          await axios.put(`https://apiproduct-nveo.onrender.com/products/${this.editedProduct.id}`, this.editedProduct);
          console.log('Producto actualizado correctamente');
          // Mostrar alerta de éxito
          Swal.fire('¡Éxito!', 'Producto actualizado exitosamente', 'success');
          this.$router.push({ path: '/home' }); // Redirige al listado de productos después de actualizar
        } catch (error) {
          console.error('Error al actualizar el producto:', error);
          // Mostrar alerta de error
          Swal.fire('Error', 'No se pudo actualizar el producto', 'error');
        }
      }
    }
  };
  </script>