<template> 
    <section id="reservaciones">
      <h2>Realizar una Reservación</h2>
      <form @submit.prevent="submitReservation">
        <div>
          <label for="nombre">Nombre completo:</label>
          <input type="text" v-model="reservation.nombre" required>
          <span v-if="errors.nombre" class="error-message">{{ errors.nombre }}</span>
        </div>
  
        <div>
          <label for="email">Correo Electrónico:</label>
          <input type="email" v-model="reservation.email" required>
          <span v-if="errors.email" class="error-message">{{ errors.email }}</span>
        </div>
  
        <div>
          <label for="tipoHabitacion">Tipo de Habitación:</label>
          <select v-model="reservation.tipoHabitacion">
            <option value="individual" data-precio="50">Habitación Simple - $50</option>
            <option value="doble" data-precio="75">Habitación Doble - $75</option>
            <option value="suite" data-precio="120">Suite - $120</option>
          </select>
        </div>
  
        <div>
          <label for="checkin">Fecha de Llegada:</label>
          <input type="date" v-model="reservation.checkin" required>
          <span v-if="errors.checkin" class="error-message">{{ errors.checkin }}</span>
        </div>
  
        <div>
          <label for="checkout">Fecha de Salida:</label>
          <input type="date" v-model="reservation.checkout" required>
          <span v-if="errors.checkout" class="error-message">{{ errors.checkout }}</span>
        </div>
  
        <div>
          <label for="numPersonas">Número de Personas:</label>
          <input type="number" v-model="reservation.numPersonas" min="1" required>
          <span v-if="errors.numPersonas" class="error-message">{{ errors.numPersonas }}</span>
        </div>
  
        <button type="submit">Reservar</button>
      </form>
  
      <!-- Ventana emergente de facturación -->
      <div v-if="showInvoice" class="popup">
        <h3>Resumen de la Reserva</h3>
        <p><strong>Nombre:</strong> {{ reservation.nombre }}</p>
        <p><strong>Correo Electrónico:</strong> {{ reservation.email }}</p>
        <p><strong>Tipo de Habitación:</strong> {{ reservation.tipoHabitacion }}</p>
        <p><strong>Fecha de Llegada:</strong> {{ reservation.checkin }}</p>
        <p><strong>Fecha de Salida:</strong> {{ reservation.checkout }}</p>
        <p><strong>Número de Personas:</strong> {{ reservation.numPersonas }}</p>
        <p><strong>Total a Pagar:</strong> ${{ calcularTotal }}</p>
        <button @click="proceedToPayment">Proceder al Pago</button>
        <button @click="closeInvoice">Cerrar</button>
      </div>
  
      <!-- Ventana emergente de pago -->
      <div v-if="showPayment" class="popup">
        <h3>Formulario de Pago</h3>
        <form @submit.prevent="submitPayment">
          <label for="cardNumber">Número de Tarjeta:</label>
          <input type="text" v-model="payment.cardNumber" placeholder="Número de tarjeta" required>
          <span v-if="errors.cardNumber" class="error-message">{{ errors.cardNumber }}</span>
  
          <label for="expiryDate">Fecha de Expiración (MM/AA):</label>
          <input type="text" v-model="payment.expiryDate" placeholder="MM/AA" required>
          <span v-if="errors.expiryDate" class="error-message">{{ errors.expiryDate }}</span>
  
          <label for="cvv">CVV:</label>
          <input type="text" v-model="payment.cvv" placeholder="CVV" required>
          <span v-if="errors.cvv" class="error-message">{{ errors.cvv }}</span>
  
          <button type="submit">Pagar</button>
          <button @click="closePayment">Cancelar</button>
        </form>
      </div>
    </section>
  </template>
  
  <script>
  import { saveReservation, initDB } from '../js/indexedDB';
  
  export default {
    name: 'ReservationFormComponent',
    data() {
      return {
        reservation: {
          nombre: '',
          email: '',
          tipoHabitacion: 'individual',
          checkin: '',
          checkout: '',
          numPersonas: 1
        },
        payment: {
          cardNumber: '',
          expiryDate: '',
          cvv: ''
        },
        errors: {
          nombre: '',
          email: '',
          checkin: '',
          checkout: '',
          numPersonas: '',
          cardNumber: '',
          expiryDate: '',
          cvv: ''
        },
        showInvoice: false,
        showPayment: false
      };
    },
    computed: {
      calcularTotal() {
        let precio = 0;
  
        switch (this.reservation.tipoHabitacion) {
          case 'individual':
            precio = 50;
            break;
          case 'doble':
            precio = 75;
            break;
          case 'suite':
            precio = 120;
            break;
        }
  
        const checkinDate = new Date(this.reservation.checkin);
        const checkoutDate = new Date(this.reservation.checkout);
        const days = (checkoutDate - checkinDate) / (1000 * 60 * 60 * 24);
  
        return Math.max(1, days) * precio;
      }
    },
    methods: {
      submitReservation() {
        this.validateForm();
  
        if (
          this.errors.nombre ||
          this.errors.email ||
          this.errors.checkin ||
          this.errors.checkout ||
          this.errors.numPersonas
        ) {
          return;
        }
  
        this.showInvoice = true;
  
        // Guardar la reservación en IndexedDB usando la función saveReservation
        const reservationData = { ...this.reservation, total: this.calcularTotal };
        saveReservation(reservationData)
          .then(() => {
            console.log("Reservación guardada exitosamente en IndexedDB");
          })
          .catch((error) => {
            console.error("Error al guardar la reservación", error);
          });
      },
      proceedToPayment() {
        this.showInvoice = false;
        this.showPayment = true;
      },
      submitPayment() {
        this.validatePayment();
  
        if (this.errors.cardNumber || this.errors.expiryDate || this.errors.cvv) {
          return;
        }
  
        alert('Pago realizado con éxito. ¡Gracias por tu reserva!');
        this.showPayment = false;
      },
      validateForm() {
        this.errors.nombre = this.reservation.nombre ? '' : 'El nombre es obligatorio.';
        this.errors.email = /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(this.reservation.email)
          ? ''
          : 'Por favor, ingrese un correo electrónico válido.';
        this.errors.checkin = this.reservation.checkin ? '' : 'La fecha de llegada es obligatoria.';
        this.errors.checkout = this.reservation.checkout
          ? new Date(this.reservation.checkout) > new Date(this.reservation.checkin)
            ? ''
            : 'La fecha de salida debe ser posterior a la fecha de llegada.'
          : 'La fecha de salida es obligatoria.';
        this.errors.numPersonas = this.reservation.numPersonas > 0 ? '' : 'Debe haber al menos una persona.';
      },
      validatePayment() {
        this.errors.cardNumber = this.payment.cardNumber.length >= 13 ? '' : 'Número de tarjeta inválido.';
        this.errors.expiryDate = /^(0[1-9]|1[0-2])\/?([0-9]{2})$/.test(this.payment.expiryDate)
          ? ''
          : 'Formato de fecha de expiración incorrecto.';
        this.errors.cvv = /^[0-9]{3,4}$/.test(this.payment.cvv) ? '' : 'CVV inválido.';
      },
      closeInvoice() {
        this.showInvoice = false;
      },
      closePayment() {
        this.showPayment = false;
      }
    },
    mounted() {
      initDB(); // Inicializar la base de datos cuando se monta el componente
    }
  };
  </script>
  
  <style scoped>
  .error-message {
    color: red;
    font-size: 0.875em;
    margin-top: 5px;
  }
  .popup {
    background-color: #fff;
    padding: 20px;
    border-radius: 5px;
    box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.2);
    margin-top: 20px;
  }
  form {
    background-color: #fff;
    padding: 20px;
    border-radius: 5px;
    box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
  }
  label {
    display: block;
    margin-bottom: 5px;
    color: #333;
  }
  input, select, button {
    width: 100%;
    padding: 10px;
    margin-bottom: 10px;
    border-radius: 5px;
    border: 1px solid #ccc;
  }
  button {
    background-color: #333;
    color: #fff;
    cursor: pointer;
  }
  button:hover {
    background-color: #555;
  }
  </style>
  