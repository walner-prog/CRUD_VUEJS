<template>
    <div class="container">

        <div class="row mt-3">
            <div class="col-md-6 offset-md-3">
                <div class="card">
                    <div class="card-header bg-dark text-white text-center">Crear Producto</div>
                    <div class="card-body">
                        <h1>Crear Producto</h1>
                        <form @submit.prevent="createProduct">
                            <div class="form-group">
                              <label for="name">Nombre:</label>
                              <input type="text" class="form-control" id="name" v-model="product.name" required>
                            </div>
                            <div class="form-group">
                              <label for="description">Descripción:</label>
                              <textarea class="form-control" id="description" v-model="product.description" required></textarea>
                            </div>
                            <div class="form-group">
                              <label for="price">Precio:</label>
                              <input type="decimal" class="form-control" id="price" v-model="product.price" required>
                            </div>
                            <div class="form-group">
                              <label for="category">Categoría:</label>
                              <input type="text" class="form-control" id="category" v-model="product.category" required>
                            </div>
                            <div class="form-group">
                              <label for="image">URL de la imagen:</label>
                              <input type="text" class="form-control" id="image" v-model="product.image" >
                            </div>
                            <br>
                            <button type="submit" class="btn btn-primary text-center">Crear Producto</button>
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
      product: {
        name: '',
        description: '',
        price: null,
        category: '',
        image: ''
      }
    };
  },
  methods: {
    async createProduct() {
      try {
        const response = await axios.post('https://apiproduct-nveo.onrender.com/products', this.product);
        console.log('Producto creado:', response.data);
        // Mostrar alerta de éxito
        Swal.fire('¡Éxito!', 'Producto creado exitosamente', 'success');
        // Redirige al listado de productos después de crear uno
        this.$router.push({ path: '/home' });
      } catch (error) {
        console.error('Error al crear el producto:', error);
        // Mostrar alerta de error
        Swal.fire('Error', 'No se pudo crear el producto', 'error');
      }
    }
  }
};
</script>