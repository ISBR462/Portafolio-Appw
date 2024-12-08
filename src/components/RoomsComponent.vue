<template>
    <section id="habitaciones">
      <h2>Habitaciones</h2>
      <ul>
        <li v-for="room in rooms" :key="room.name">
          <img :src="require(`@/assets/${room.image}`)" :alt="room.name">
          <strong>{{ room.name }}:</strong> {{ room.description }} - ${{ room.price }} por noche
        </li>
      </ul>
    </section>
  </template>
  
  <script>
  export default {
    name: 'RoomsComponent',
    data() {
      return {
        rooms: [
          { name: 'Habitación Individual', price: 50, description: 'Ideal para una persona', image: 'individual.jpg' },
          { name: 'Habitación Doble', price: 75, description: 'Perfecta para dos personas', image: 'doble.jpg' },
          { name: 'Suite', price: 120, description: 'Con todas las comodidades', image: 'suite.jpg' }
        ]
      }
    }
  }
  </script>
  
  <style scoped>
  ul {
    list-style: none;
    padding: 0;
  }
  li {
    display: flex;
    align-items: center;
    margin-bottom: 20px;
  }
  img {
    width: 150px;
    margin-right: 20px;
    border-radius: 8px;
  }
  </style>
  