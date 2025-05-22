<template>
    <div v-if="showPaymentForm" class="popup">
      <h3>Formulario de Pago</h3>
      <form @submit.prevent="submitPayment">
        <label for="cardNumber">Número de Tarjeta:</label>
        <input type="text" v-model="cardNumber" placeholder="Ingresar el Númeroo de tarjeta" required>
  
        <label for="expiryDate">Fecha de Expiración (MM/AA):</label>
        <input type="text" v-model="expiryDate" placeholder="MM/AA" required>
  
        <label for="cvv">CVV:</label>
        <input type="text" v-model="cvv" placeholder="CVV" required>
  
        <button type="submit">Pagar</button>
      </form>
    </div>
  </template>
  
  <script>
  export default {
    name: 'PaymentComponent',
    props: ['showPaymentForm'],
    data() {
      return {
        cardNumber: '',
        expiryDate: '',
        cvv: ''
      };
    },
    methods: {
      submitPayment() {
        // Aquí podrías agregar la lógica para procesar el pago
        alert('Pago realizado con éxito. ¡Gracias por tu reserva!');
        this.$emit('payment-complete'); // Emite un evento para notificar que el pago se completó
      }
    }
  };
  </script>
  
  <style scoped>
  .popup {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    padding: 20px;
    background-color: #fff;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    z-index: 1000;
  }
  
  .popup input {
    width: 100%;
    padding: 10px;
    margin-bottom: 15px;
    border: 1px solid #ccc;
    border-radius: 5px;
  }
  
  .popup button {
    width: 100%;
    padding: 10px;
    background-color: #333;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }
  
  .popup button:hover {
    background-color: #555;
  }
  </style>
  
  