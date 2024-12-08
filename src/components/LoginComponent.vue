<template>
    <div v-if="!isAuthenticated" class="login-window">
      <h2>Iniciar Sesión</h2>
      <form @submit.prevent="handleLogin">
        <label for="email">Correo Electrónico:</label>
        <input type="email" v-model="email" required>
  
        <label for="password">Contraseña:</label>
        <input type="password" v-model="password" required>
  
        <button type="submit">Iniciar Sesión</button>
        <p v-if="errorMessage" style="color: red;">{{ errorMessage }}</p>
      </form>
    </div>
  </template>
  
  <script>
  export default {
    name: 'LoginComponent',
    data() {
      return {
        email: '',
        password: '',
        errorMessage: '',
        isAuthenticated: false
      };
    },
    methods: {
      handleLogin() {
        // Validar que el correo tenga el formato correcto
        const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
        if (!emailPattern.test(this.email)) {
          this.errorMessage = 'Por favor, ingresa un correo electrónico válido.';
          return;
        }
  
        // Validar que la contraseña cumpla ciertos parámetros
        // Por ejemplo, al menos 6 caracteres
        if (this.password.length < 6) {
          this.errorMessage = 'La contraseña debe tener al menos 6 caracteres.';
          return;
        }
  
        // Si todas las validaciones pasan, el usuario está autenticado
        this.isAuthenticated = true;
        this.$emit('authenticated'); // Emitimos el evento al componente padre para que maneje la autenticación
      }
    }
  };
  </script>
  
  <style scoped>
  .login-window {
    max-width: 400px;
    margin: 100px auto;
    padding: 20px;
    background: #fff;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    text-align: center;
  }
  .login-window h2 {
    margin-bottom: 15px;
  }
  .login-window label {
    display: block;
    text-align: left;
    margin-bottom: 5px;
  }
  .login-window input {
    width: 100%;
    padding: 10px;
    margin-bottom: 15px;
    border: 1px solid #ccc;
    border-radius: 5px;
  }
  .login-window button {
    width: 100%;
    padding: 10px;
    background-color: #333;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }
  .login-window button:hover {
    background-color: #555;
  }
  </style>
  