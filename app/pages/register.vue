<template>
  <main>
    <RegisterForm @register-success="handleRegisterSuccess" />
  </main>
</template>

<script setup lang="ts">
const handleRegisterSuccess = (userData: any) => {
  console.log('Registration successful in page:', userData)
  alert('Kayıt başarılı! Giriş yapabilirsiniz.')
  navigateTo('/')
}
</script>

<style scoped>
main {
  width: 100%;
  height: 100%;
}
</style>
