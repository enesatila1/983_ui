<template>
  <div class="login-wrapper">
    <div class="login-box">
      <div class="login-header">
        <div class="logo-placeholder">
          <span class="logo-icon">🔒</span>
        </div>
        <h1 class="login-title">Hoş Geldiniz</h1>
        <p class="login-subtitle">Devam etmek için lütfen giriş yapın</p>
      </div>

      <form @submit.prevent="handleSubmit" class="login-form">
        <div class="form-field">
          <label for="email">E-posta Adresi</label>
          <div class="input-container">
            <span class="input-icon">✉️</span>
            <input
              id="email"
              v-model="email"
              type="email"
              placeholder="admin@admin.com"
              required
              class="form-input"
              :class="{ 'has-error': error && !email }"
            />
          </div>
        </div>
        
        <div class="form-field">
          <div class="label-row">
            <label for="password">Şifre</label>
            <a href="#" class="forgot-password">Şifremi Unuttum?</a>
          </div>
          <div class="input-container">
            <span class="input-icon">🔑</span>
            <input
              id="password"
              v-model="password"
              type="password"
              placeholder="••••••••"
              required
              class="form-input"
              :class="{ 'has-error': error && !password }"
            />
          </div>
        </div>

        <div class="form-options">
          <label class="remember-me">
            <input type="checkbox" v-model="rememberMe" />
            <span>Beni Hatırla</span>
          </label>
        </div>

        <div v-show="error" class="error-box">
          <span class="error-icon">⚠️</span>
          {{ error }}
        </div>

        <button :disabled="isLoading" type="submit" class="submit-btn">
          <span v-if="isLoading" class="loader"></span>
          <span v-else>Giriş Yap</span>
        </button>

        <div class="divider">
          <span>VEYA</span>
        </div>

        <div class="social-login">
          <button type="button" class="social-btn google-btn">
            <span class="social-icon">G</span> Google ile Giriş
          </button>
          <button type="button" class="social-btn github-btn">
            <span class="social-icon">H</span> GitHub ile Giriş
          </button>
        </div>
      </form>

      <div class="login-footer">
        <p>Hesabınız yok mu? <NuxtLink to="/register" class="signup-link">Kayıt Ol</NuxtLink></p>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const email = ref('')
const password = ref('')
const rememberMe = ref(false)
const error = ref('')
const isLoading = ref(false)

const emit = defineEmits(['login-success', 'login-error'])

const handleSubmit = async () => {
  if (!email.value || !password.value) {
    error.value = 'Lütfen tüm alanları doldurun.'
    return
  }

  error.value = ''
  isLoading.value = true

  try {
    // API çağrısı simülasyonu
    console.log('Giriş isteği:', { 
      email: email.value, 
      password: password.value, 
      rememberMe: rememberMe.value 
    })
    
    await new Promise(resolve => setTimeout(resolve, 1500))

    if (email.value === 'admin@admin.com' && password.value === '123456') {
      emit('login-success', { email: email.value })
    } else {
      error.value = 'E-posta veya şifre hatalı!'
      emit('login-error', error.value)
    }
  } catch (err) {
    error.value = 'Sunucuyla iletişim kurulamadı.'
  } finally {
    isLoading.value = false
  }
}
</script>

<style scoped>
.login-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background: linear-gradient(rgba(0, 0, 0, 0.6), rgba(0, 0, 0, 0.6)), 
              url('https://images7.alphacoders.com/133/1338637.png');
  background-size: cover;
  background-position: center;
  padding: 1rem;
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
}

.login-box {
  background: rgba(255, 255, 255, 0.95);
  padding: 2.5rem;
  border-radius: 1.25rem;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.3);
  width: 100%;
  max-width: 440px;
  animation: fadeIn 0.5s ease-out;
  border: 2px solid #5d4037;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}

.login-header {
  text-align: center;
  margin-bottom: 2rem;
}

.logo-placeholder {
  width: 64px;
  height: 64px;
  background: #5d4037;
  border-radius: 1rem;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto 1.25rem;
  color: white;
  font-size: 1.5rem;
  box-shadow: 0 4px 12px rgba(93, 64, 55, 0.3);
}

.login-title {
  font-size: 1.75rem;
  font-weight: 800;
  color: #3e2723;
  margin-bottom: 0.5rem;
}

.login-subtitle {
  color: #5d4037;
  font-size: 0.9375rem;
}

.login-form {
  display: flex;
  flex-direction: column;
  gap: 1.25rem;
}

.form-field {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.label-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.form-field label {
  font-size: 0.875rem;
  font-weight: 600;
  color: #3e2723;
}

.forgot-password, .signup-link {
  font-size: 0.8125rem;
  color: #795548;
  text-decoration: none;
  font-weight: 600;
}

.forgot-password:hover, .signup-link:hover {
  text-decoration: underline;
  color: #3e2723;
}

.input-container {
  position: relative;
  display: flex;
  align-items: center;
}

.input-icon {
  position: absolute;
  left: 1rem;
  font-size: 1rem;
}

.form-input {
  width: 100%;
  padding: 0.75rem 1rem 0.75rem 2.75rem;
  border: 1.5px solid #d7ccc8;
  border-radius: 0.75rem;
  font-size: 0.9375rem;
  transition: all 0.2s;
  background-color: #efebe9;
  color: #3e2723;
}

.form-input:focus {
  outline: none;
  border-color: #5d4037;
  background-color: white;
  box-shadow: 0 0 0 4px rgba(93, 64, 55, 0.1);
}

.form-input.has-error {
  border-color: #ef4444;
}

.form-options {
  display: flex;
  align-items: center;
}

.remember-me {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.875rem;
  color: #5d4037;
  cursor: pointer;
}

.remember-me input {
  width: 16px;
  height: 16px;
  border-radius: 4px;
  cursor: pointer;
}

.error-box {
  background-color: #fbe9e7;
  border: 1px solid #ffccbc;
  color: #d84315;
  padding: 0.75rem;
  border-radius: 0.75rem;
  font-size: 0.875rem;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.submit-btn {
  padding: 0.875rem;
  background: #5d4037;
  color: white;
  border: none;
  border-radius: 0.75rem;
  font-weight: 700;
  font-size: 1rem;
  cursor: pointer;
  transition: all 0.2s;
  display: flex;
  justify-content: center;
  align-items: center;
  box-shadow: 0 4px 6px rgba(93, 64, 55, 0.2);
}

.submit-btn:hover:not(:disabled) {
  background: #4e342e;
  transform: translateY(-1px);
  box-shadow: 0 6px 12px rgba(93, 64, 55, 0.25);
}

.submit-btn:active:not(:disabled) {
  transform: translateY(0);
}

.submit-btn:disabled {
  background: #bcaaa4;
  cursor: not-allowed;
  box-shadow: none;
}

.divider {
  display: flex;
  align-items: center;
  text-align: center;
  margin: 0.5rem 0;
}

.divider::before, .divider::after {
  content: '';
  flex: 1;
  border-bottom: 1px solid #d7ccc8;
}

.divider span {
  padding: 0 1rem;
  color: #8d6e63;
  font-size: 0.75rem;
  font-weight: 600;
}

.social-login {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
}

.social-btn {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  padding: 0.625rem;
  border: 1.5px solid #d7ccc8;
  border-radius: 0.75rem;
  background: white;
  color: #3e2723;
  font-size: 0.8125rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s;
}

.social-btn:hover {
  background: #efebe9;
  border-color: #bcaaa4;
}

.login-footer {
  margin-top: 2rem;
  text-align: center;
  font-size: 0.875rem;
  color: #5d4037;
}

.loader {
  width: 20px;
  height: 20px;
  border: 3px solid rgba(255, 255, 255, 0.3);
  border-radius: 50%;
  border-top-color: white;
  animation: spin 0.8s linear infinite;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}
</style>
