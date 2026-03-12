<template>
  <div class="register-wrapper">
    <div class="register-box">
      <div class="register-header">
        <div class="logo-placeholder">
          <span class="logo-icon">📝</span>
        </div>
        <h1 class="register-title">Kayıt Ol</h1>
        <p class="register-subtitle">Aramıza katılmak için formu doldurun</p>
      </div>

      <form @submit.prevent="handleSubmit" class="register-form">
        <div class="form-field">
          <label for="fullName">Ad Soyad</label>
          <div class="input-container">
            <span class="input-icon">👤</span>
            <input
              id="fullName"
              v-model="fullName"
              type="text"
              placeholder="Adınız Soyadınız"
              required
              class="form-input"
              :class="{ 'has-error': error && !fullName }"
            />
          </div>
        </div>

        <div class="form-field">
          <label for="email">E-posta Adresi</label>
          <div class="input-container">
            <span class="input-icon">✉️</span>
            <input
              id="email"
              v-model="email"
              type="email"
              placeholder="örnek@eposta.com"
              required
              class="form-input"
              :class="{ 'has-error': error && !email }"
            />
          </div>
        </div>
        
        <div class="form-field">
          <label for="password">Şifre</label>
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

        <div class="form-field">
          <label for="confirmPassword">Şifre Tekrar</label>
          <div class="input-container">
            <span class="input-icon">🔒</span>
            <input
              id="confirmPassword"
              v-model="confirmPassword"
              type="password"
              placeholder="••••••••"
              required
              class="form-input"
              :class="{ 'has-error': error && (password !== confirmPassword) }"
            />
          </div>
        </div>

        <div v-show="error" class="error-box">
          <span class="error-icon">⚠️</span>
          {{ error }}
        </div>

        <button :disabled="isLoading" type="submit" class="submit-btn">
          <span v-if="isLoading" class="loader"></span>
          <span v-else>Kayıt Ol</span>
        </button>

        <div class="divider">
          <span>VEYA</span>
        </div>

        <div class="social-login">
          <button type="button" class="social-btn google-btn">
            <span class="social-icon">G</span> Google ile Kayıt
          </button>
          <button type="button" class="social-btn github-btn">
            <span class="social-icon">H</span> GitHub ile Kayıt
          </button>
        </div>
      </form>

      <div class="register-footer">
        <p>Zaten hesabınız var mı? <NuxtLink to="/" class="login-link">Giriş Yap</NuxtLink></p>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const fullName = ref('')
const email = ref('')
const password = ref('')
const confirmPassword = ref('')
const error = ref('')
const isLoading = ref(false)

const emit = defineEmits(['register-success', 'register-error'])

const handleSubmit = async () => {
  if (!fullName.value || !email.value || !password.value || !confirmPassword.value) {
    error.value = 'Lütfen tüm alanları doldurun.'
    return
  }

  if (password.value !== confirmPassword.value) {
    error.value = 'Şifreler eşleşmiyor.'
    return
  }

  error.value = ''
  isLoading.value = true

  try {
    // API çağrısı simülasyonu
    console.log('Kayıt isteği:', { 
      fullName: fullName.value,
      email: email.value, 
      password: password.value
    })
    
    await new Promise(resolve => setTimeout(resolve, 1500))

    emit('register-success', { email: email.value, fullName: fullName.value })
  } catch (err) {
    error.value = 'Sunucuyla iletişim kurulamadı.'
    emit('register-error', error.value)
  } finally {
    isLoading.value = false
  }
}
</script>

<style scoped>
.register-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
  padding: 1rem;
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
}

.register-box {
  background: white;
  padding: 2.5rem;
  border-radius: 1.25rem;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.05);
  width: 100%;
  max-width: 440px;
  animation: fadeIn 0.5s ease-out;
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(10px); }
  to { opacity: 1; transform: translateY(0); }
}

.register-header {
  text-align: center;
  margin-bottom: 2rem;
}

.logo-placeholder {
  width: 64px;
  height: 64px;
  background: #10b981;
  border-radius: 1rem;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto 1.25rem;
  color: white;
  font-size: 1.5rem;
  box-shadow: 0 4px 12px rgba(16, 185, 129, 0.3);
}

.register-title {
  font-size: 1.75rem;
  font-weight: 800;
  color: #1f2937;
  margin-bottom: 0.5rem;
}

.register-subtitle {
  color: #6b7280;
  font-size: 0.9375rem;
}

.register-form {
  display: flex;
  flex-direction: column;
  gap: 1.25rem;
}

.form-field {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.form-field label {
  font-size: 0.875rem;
  font-weight: 600;
  color: #374151;
}

.login-link {
  font-size: 0.8125rem;
  color: #10b981;
  text-decoration: none;
  font-weight: 600;
}

.login-link:hover {
  text-decoration: underline;
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
  border: 1.5px solid #e5e7eb;
  border-radius: 0.75rem;
  font-size: 0.9375rem;
  transition: all 0.2s;
  background-color: #f9fafb;
}

.form-input:focus {
  outline: none;
  border-color: #10b981;
  background-color: white;
  box-shadow: 0 0 0 4px rgba(16, 185, 129, 0.1);
}

.form-input.has-error {
  border-color: #ef4444;
}

.error-box {
  background-color: #fef2f2;
  border: 1px solid #fee2e2;
  color: #b91c1c;
  padding: 0.75rem;
  border-radius: 0.75rem;
  font-size: 0.875rem;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.submit-btn {
  padding: 0.875rem;
  background: #10b981;
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
  box-shadow: 0 4px 6px rgba(16, 185, 129, 0.2);
}

.submit-btn:hover:not(:disabled) {
  background: #059669;
  transform: translateY(-1px);
  box-shadow: 0 6px 12px rgba(16, 185, 129, 0.25);
}

.submit-btn:active:not(:disabled) {
  transform: translateY(0);
}

.submit-btn:disabled {
  background: #a7f3d0;
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
  border-bottom: 1px solid #e5e7eb;
}

.divider span {
  padding: 0 1rem;
  color: #9ca3af;
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
  border: 1.5px solid #e5e7eb;
  border-radius: 0.75rem;
  background: white;
  color: #374151;
  font-size: 0.8125rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s;
}

.social-btn:hover {
  background: #f9fafb;
  border-color: #d1d5db;
}

.register-footer {
  margin-top: 2rem;
  text-align: center;
  font-size: 0.875rem;
  color: #6b7280;
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
