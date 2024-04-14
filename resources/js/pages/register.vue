<template>
    <div>
    <div class="min-h-screen flex flex-col justify-center items-center bg-gray-100">
      <div class="max-w-md w-full mx-auto bg-white p-8 border border-gray-300">
        <form @submit.prevent="register" class="space-y-6">
          <h2 class="text-2xl font-semibold text-center text-gray-700">Inscription</h2>
          <div>
            <label for="name" class="text-sm font-medium text-gray-700">Nom</label>
            <input type="text" id="name" v-model="name"
                   class="mt-1 w-full p-2 border border-gray-300 rounded-md"
                   required>
          </div>
          <div>
            <label for="email" class="text-sm font-medium text-gray-700">Email</label>
            <input type="email" id="email" v-model="email"
                   class="mt-1 w-full p-2 border border-gray-300 rounded-md"
                   placeholder="votreemail@example.com" required>
          </div>
          <div>
            <label for="password" class="text-sm font-medium text-gray-700">Mot de passe</label>
            <input type="password" id="password" v-model="password"
                   class="mt-1 w-full p-2 border border-gray-300 rounded-md"
                   required>
          </div>
          <div>
            <button type="submit"
                    class="w-full flex justify-center py-2 px-4 border border-transparent 
                           rounded-md shadow-sm text-sm font-medium text-white 
                           bg-green-600 hover:bg-green-700 focus:outline-none focus:ring-2 
                           focus:ring-offset-2 focus:ring-green-500">
              S'inscrire
            </button>
          </div>
        </form>
      </div>
    </div>
</div>
  </template>
  
  <script>
import axios from 'axios';

export default {
  data() {
    return {
      name: '',
      email: '',
      password: '',
      errorMessage: '', 
    };
  },
  methods: {
    async register() {
      this.errorMessage = '';

      try {
        const response = await axios.post('http://127.0.0.1:8000/api/auth/register', {
          name: this.name,
          email: this.email,
          password: this.password,
        });
        console.log(response.data); 
        localStorage.setItem('authToken', response.data.access_token);

        this.$router.replace('/home');
        alert('Inscription réussie.');
      } catch (error) {
        if (error.response && error.response.data) {
          console.error(error.response.data);
          this.errorMessage = error.response.data.message || 'Une erreur est survenue lors de l’inscription.';
        } else {
          this.errorMessage = 'Erreur de réseau ou serveur non disponible.';
        }
      }
    },
  },
};
</script>

  