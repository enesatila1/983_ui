<template>
  <div class="login-container">
    <div class="login-card">
      <h1 class="login-title">Giriş Yap</h1>
      <form @submit.prevent="handleSubmit" class="login-form">
        <div class="form-group">
          <label for="email">E-posta</label>
          <input
            id="email"
            v-model="email"
            type="email"
            placeholder="örnek@eposta.com"
            required
            class="form-input"
          />
        </div>
        
        <div class="form-group">
          <label for="password">Şifre</label>
          <input
            id="password"
            v-model="password"
            type="password"
            placeholder="••••••••"
            required
            class="form-input"
          />
        </div>

        <div v-if="error" class="error-message">
          {{ error }}
        </div>

        <button :disabled="isLoading" type="submit" class="login-button">
          <span v-if="isLoading">Giriş yapılıyor...</span>
          <span v-else>Giriş Yap</span>
        </button>
      </form>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const email = ref('')
const password = ref('')
const error = ref('')
const isLoading = ref(false)

const emit = defineEmits(['login-success', 'login-error'])

const handleSubmit = async () => {
  error.value = ''
  isLoading.value = true

  // Bu kısım simüle edilmiştir
  try {
    // Burada servis çağrısı yapılabilir (örn: shared/services/auth)
    console.log('Giriş bilgileri:', { email: email.value, password: password.value })
    
    // Simüle edilen bekleme süresi
    await new Promise(resolve => setTimeout(resolve, 1000))

    if (email.value === 'admin@admin.com' && password.value === '123456') {
      emit('login-success', { email: email.value })
    } else {
      error.value = 'E-posta veya şifre hatalı!'
      emit('login-error', error.value)
    }
  } catch (err) {
    error.value = 'Bir hata oluştu. Lütfen tekrar deneyin.'
  } finally {
    isLoading.value = false
  }
}
</script>

<style scoped>
.login-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background-color: #f3f4f6;
}

.login-card {
  background: white;
  padding: 2rem;
  border-radius: 8px;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
  width: 100%;
  max-width: 400px;
}

.login-title {
  font-size: 1.5rem;
  font-weight: 700;
  text-align: center;
  margin-bottom: 2rem;
  color: #1f2937;
}

.login-form {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.form-group label {
  font-size: 0.875rem;
  font-weight: 500;
  color: #374151;
}

.form-input {
  padding: 0.625rem;
  border: 1px solid #d1d5db;
  border-radius: 4px;
  font-size: 1rem;
}

.form-input:focus {
  outline: 2px solid #3b82f6;
  border-color: transparent;
}

.login-button {
  padding: 0.75rem;
  background-color: #3b82f6;
  color: white;
  border: none;
  border-radius: 4px;
  font-weight: 600;
  cursor: pointer;
  transition: background-color 0.2s;
}

.login-button:hover:not(:disabled) {
  background-color: #2563eb;
}

.login-button:disabled {
  background-color: #93c5fd;
  cursor: not-allowed;
}

.error-message {
  color: #ef4444;
  font-size: 0.875rem;
  text-align: center;
}
</style>
