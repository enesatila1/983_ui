<template>
  <main>
    <LoginForm @login-success="handleLoginSuccess" />
  </main>
</template>

<script setup lang="ts">
// Nuxt 3/4 auto-imports components from components/ directory by default.
// If it doesn't, you can import it: import LoginForm from '~/components/LoginForm.vue'

const handleLoginSuccess = (userData: any) => {
  console.log('Login successful in page:', userData)
  // Burada yönlendirme yapılabilir: useRouter().push('/dashboard')
  alert('Giriş başarılı! Hoş geldiniz, ' + userData.email)
}
</script>

<style scoped>
/* Page specific styles if needed */
main {
  width: 100%;
  height: 100%;
}
</style>
