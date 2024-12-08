*<template>
    <header>
      <h1>Hotel ULEAM - Gestión Hotelera</h1>
      <nav>
        <ul>
          <li><a href="#reservaciones">Reservaciones</a></li>
          <li><a href="#habitaciones">Habitaciones</a></li>
          <li><a href="#contacto">Contacto</a></li>
        </ul>
      </nav>
    </header>
  </template>
  
  <script>
  export default {
    name: 'HeaderComponent'
  }
  </script>
  
  <style scoped>
  header {
    background-color: #333;
    color: #fff;
    padding: 10px;
    text-align: center;
  }
  nav ul {
    list-style: none;
    padding: 0;
  }
  nav ul li {
    display: inline;
    margin-right: 15px;
  }
  nav ul li a {
    color: #fff;
    text-decoration: none;
  }
  </style>
  