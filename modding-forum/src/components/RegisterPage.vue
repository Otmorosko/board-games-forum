<template>
  <div>
    <h2>Register</h2>
    <form @submit.prevent="register">
      <div>
        <label for="username">Username:</label>
        <input type="text" v-model="username" required />
      </div>
      <div>
        <label for="email">Email:</label>
        <input type="email" v-model="email" required />
      </div>
      <div>
        <label for="password">Password:</label>
        <input type="password" v-model="password" required />
      </div>
      <button type="submit">Register</button>
    </form>
    <p v-if="message">{{ message }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      username: '',
      email: '',
      password: '',
      message: ''
    };
  },
  methods: {
    async register() {
      try {
        console.log('Wysyłam dane rejestracji:', {
          username: this.username,
          email: this.email,
          password: this.password
        });

        const response = await fetch('http://localhost:3000/api/users/register', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({
            username: this.username,
            email: this.email,
            password: this.password
          })
        });

        console.log('Otrzymana odpowiedź z serwera:', response);

        if (response.ok) {
          this.message = 'User registered successfully!';
          console.log('Rejestracja zakończona sukcesem');
        } else {
          const errorData = await response.json();
          console.log('Błąd rejestracji:', errorData);
          this.message = errorData.message || 'Registration failed.';
        }
      } catch (error) {
        console.error('Wystąpił błąd:', error);
        this.message = 'An error occurred: ' + error.message;
      }
    }
  }
};
</script>

<style scoped>
/* Możesz dodać tutaj stylizację */
</style>
