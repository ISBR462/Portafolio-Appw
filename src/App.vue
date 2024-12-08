<template>
  <div id="app">
    <LoginComponent v-if="!isAuthenticated" @authenticated="handleAuthentication" />
    <div v-else>
      <HeaderComponent />
      <main>
        <ReservationFormComponent @proceed-to-payment="handleProceedToPayment" />
        <RoomsComponent />
        <ContactComponent />
      </main>
      <FooterComponent />
      <PaymentComponent v-if="showPaymentForm" :showPaymentForm="showPaymentForm" @payment-complete="handlePaymentComplete" />
    </div>
  </div>
</template>

<script>
import HeaderComponent from './components/HeaderComponent.vue';
import ReservationFormComponent from './components/ReservationFormComponent.vue';
import RoomsComponent from './components/RoomsComponent.vue';
import ContactComponent from './components/ContactComponent.vue';
import FooterComponent from './components/FooterComponent.vue';
import LoginComponent from './components/LoginComponent.vue';
import PaymentComponent from './components/PaymentComponent.vue';

export default {
  name: 'App',
  components: {
    HeaderComponent,
    ReservationFormComponent,
    RoomsComponent,
    ContactComponent,
    FooterComponent,
    LoginComponent,
    PaymentComponent
  },
  data() {
    return {
      isAuthenticated: false,
      showPaymentForm: false
    };
  },
  methods: {
    handleAuthentication() {
      this.isAuthenticated = true;
    },
    handleProceedToPayment() {
      this.showPaymentForm = true;
    },
    handlePaymentComplete() {
      this.showPaymentForm = false;
      alert('¡Gracias por tu pago! Tu reservación ha sido completada.');
    }
  }
}
</script>

<style>
body {
  font-family: Arial, sans-serif;
  background-color: #f4f4f4;
  color: #333;
}
main {
  padding: 20px;
}
</style>
