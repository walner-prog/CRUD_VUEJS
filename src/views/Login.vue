<!-- Componente Login.vue -->
<template>
    <div class="container">

        <div class="row mt-3">
            <div class="col-md-6 offset-md-3">
                <div class="card">
                    <div class="card-header bg-dark text-white text-center">Crear Producto</div>
                    <div class="card-body">
                        <h2>Iniciar Sesión</h2>
                        <form @submit.prevent="login">
                            <div class="form-group">
                                <input class=" form-control" type="text" v-model="username" placeholder="Nombre de usuario">
                            </div>
                            <div class="form-group">
                                <input class=" form-control" type="password" v-model="password" placeholder="Contraseña">
                            </div>
                          br
                          <div class="form-group justify-content-center text-center">
                            <button class="btn btn-primary text-center" type="submit">Iniciar Sesión</button>
                          </div>
                         
                        </form>
                    </div>
                </div>
            </div>
        </div>
     
    </div>
    
  </template>
  
  <script>
  export default {
    data() {
      return {
        username: '',
        password: ''
      };
    },
    methods: {
      login() {
        // Aquí debes implementar la lógica de autenticación
        if (this.username === 'usuario' && this.password === 'contraseña') {
          // Autenticación exitosa, guardar el estado de autenticación
          localStorage.setItem('loggedIn', true);
          // Redireccionar a la página principal
          this.$router.push('/home');
        } else {
          // Autenticación fallida, mostrar mensaje de error
          alert('Nombre de usuario o contraseña incorrectos');
        }
      }
    }
  };
  </script>
  