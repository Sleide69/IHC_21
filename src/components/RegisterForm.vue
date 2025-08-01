
<template>
  <div class="login-container">
    <div class="login-card">
      <div class="login-header">
        <img src="@/assets/logo.png" alt="Logo" class="login-logo" />
        <h1 class="login-title">EcoVecinos</h1>
        <p class="login-subtitle">{{ t('registerWelcome') }}</p>
      </div>

      <form @submit.prevent="handleRegister" class="login-form">
        <div class="form-group">
          <label for="nombre" class="form-label">
            <i class="fas fa-user"></i>
            {{ t('fullName') }}
          </label>
          <input
            id="nombre"
            v-model="formData.nombre"
            type="text"
            class="form-input"
            :placeholder="t('fullNamePlaceholder')"
            required
          />
          <span v-if="errors.nombre" class="error-message">{{ errors.nombre }}</span>
        </div>

        <div class="form-group">
          <label for="email" class="form-label">
            <i class="fas fa-envelope"></i>
            {{ t('email') }}
          </label>
          <input
            id="email"
            v-model="formData.email"
            type="email"
            class="form-input"
            :placeholder="t('emailPlaceholder')"
            required
          />
          <span v-if="errors.email" class="error-message">{{ errors.email }}</span>
        </div>

        <div class="form-group">
          <label for="password" class="form-label">
            <i class="fas fa-lock"></i>
            {{ t('password') }}
          </label>
          <div class="password-input-container">
            <input
              id="password"
              v-model="formData.password"
              :type="showPassword ? 'text' : 'password'"
              class="form-input"
              :placeholder="t('passwordPlaceholder')"
              required
            />
            <button
              type="button"
              @click="togglePasswordVisibility"
              class="password-toggle-btn"
              :title="showPassword ? t('hidePassword') : t('showPassword')"
            >
              <i :class="showPassword ? 'fas fa-eye-slash' : 'fas fa-eye'"></i>
            </button>
          </div>
          <span v-if="errors.password" class="error-message">{{ errors.password }}</span>
        </div>

        <div class="form-group">
          <label for="confirmPassword" class="form-label">
            <i class="fas fa-lock"></i>
            {{ t('confirmPassword') }}
          </label>
          <div class="password-input-container">
            <input
              id="confirmPassword"
              v-model="formData.confirmPassword"
              :type="showConfirmPassword ? 'text' : 'password'"
              class="form-input"
              :placeholder="t('confirmPasswordPlaceholder')"
              required
            />
            <button
              type="button"
              @click="toggleConfirmPasswordVisibility"
              class="password-toggle-btn"
              :title="showConfirmPassword ? t('hidePassword') : t('showPassword')"
            >
              <i :class="showConfirmPassword ? 'fas fa-eye-slash' : 'fas fa-eye'"></i>
            </button>
          </div>
          <span v-if="errors.confirmPassword" class="error-message">{{ errors.confirmPassword }}</span>
        </div>

        <button type="submit" class="btn-login" :disabled="isSubmitting">
          <i v-if="isSubmitting" class="fas fa-spinner fa-spin"></i>
          <i v-else class="fas fa-user-plus"></i>
          {{ isSubmitting ? t('registering') : t('registerButton') }}
        </button>

        <div class="login-divider">
          <span>{{ t('or') }}</span>
        </div>

        <router-link to="/" class="btn-register">
          <i class="fas fa-sign-in-alt"></i>
          {{ t('loginButton') }}
        </router-link>
      </form>

      <div class="form-footer">
        <div class="language-selector">
          <button @click="changeLanguage('es')" :class="{ active: currentLanguage === 'es' }">
            <span class="flag">🇪🇸</span>
            ES
          </button>
          <button @click="changeLanguage('en')" :class="{ active: currentLanguage === 'en' }">
            <span class="flag">🇺🇸</span>
            EN
          </button>
        </div>

        <div class="theme-toggle">
          <button @click="toggleTheme" class="theme-btn">
            <i :class="currentTheme === 'dark' ? 'fas fa-sun' : 'fas fa-moon'"></i>
            {{ currentTheme === 'dark' ? t('lightMode') : t('darkMode') }}
          </button>
        </div>
      </div>
    </div>
  </div>
</template>


<style scoped>
.login-container {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background: var(--bg-secondary);
  padding: 20px;
}

.login-card {
  background: var(--bg-primary);
  border-radius: 20px;
  box-shadow: var(--shadow-lg);
  padding: 40px;
  width: 100%;
  max-width: 450px;
  animation: slideUp 0.6s ease-out;
  border: 1px solid var(--border-color);
}

@keyframes slideUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.login-header {
  text-align: center;
  margin-bottom: 40px;
}

.login-logo {
  width: 80px;
  height: 80px;
  margin-bottom: 20px;
  border-radius: 50%;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
}

.login-title {
  font-size: 2.5rem;
  font-weight: 700;
  color: #2c3e50;
  margin-bottom: 10px;
}

.login-subtitle {
  color: #7f8c8d;
  font-size: 1.1rem;
  margin: 0;
}

.login-form {
  display: flex;
  flex-direction: column;
  gap: 25px;
}

.form-group {
  display: flex;
  flex-direction: column;
}

.form-label {
  display: flex;
  align-items: center;
  gap: 8px;
  font-weight: 600;
  color: #2c3e50;
  margin-bottom: 8px;
  font-size: 0.9rem;
}

.form-input {
  width: 100%;
  padding: 15px 20px;
  border: 2px solid #e8ecef;
  border-radius: 12px;
  font-size: 1rem;
  transition: all 0.3s ease;
  background: #f8f9fa;
  box-sizing: border-box;
}

.form-input:focus {
  outline: none;
  border-color: #28a745;
  background: white;
  box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.1);
}

.form-input.error {
  border-color: #dc3545;
  box-shadow: 0 0 0 2px rgba(220, 53, 69, 0.2);
}

.error-message {
  color: #dc3545;
  font-size: 0.875rem;
  margin-top: 0.25rem;
}

.btn-login {
  width: 100%;
  background: linear-gradient(135deg, #28a745 0%, #20c997 100%);
  color: white;
  border: none;
  border-radius: 12px;
  padding: 16px 24px;
  font-size: 1.1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
}

.btn-login:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 8px 25px rgba(40, 167, 69, 0.3);
}

.btn-login:disabled {
  opacity: 0.7;
  cursor: not-allowed;
}

.login-divider {
  text-align: center;
  position: relative;
}

.login-divider::before {
  content: '';
  position: absolute;
  top: 50%;
  left: 0;
  right: 0;
  height: 1px;
  background: #e8ecef;
}

.login-divider span {
  background: white;
  padding: 0 20px;
  color: #6c757d;
  font-size: 0.9rem;
}

.btn-register {
  width: 100%;
  background: transparent;
  color: #28a745;
  border: 2px solid #28a745;
  border-radius: 12px;
  padding: 16px 24px;
  font-size: 1.1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  text-decoration: none;
}

.btn-register:hover {
  background: #28a745;
  color: white;
  transform: translateY(-2px);
}

.language-selector {
  display: flex;
  gap: 10px;
  justify-content: center;
  margin-top: 30px;
  padding-top: 30px;
  border-top: 1px solid #e8ecef;
}

.language-selector button {
  display: flex;
  align-items: center;
  gap: 5px;
  padding: 8px 16px;
  border: 2px solid #e8ecef;
  border-radius: 8px;
  background: white;
  cursor: pointer;
  transition: all 0.3s ease;
  font-weight: 500;
}

.language-selector button.active {
  border-color: #28a745;
  color: #28a745;
}

.flag {
  font-size: 1.2em;
}

.password-input-container {
  position: relative;
  display: flex;
  align-items: center;
}

.password-toggle-btn {
  position: absolute;
  right: 12px;
  background: none;
  border: none;
  cursor: pointer;
  color: #6c757d;
  font-size: 1rem;
  padding: 4px;
  border-radius: 4px;
  transition: color 0.2s ease;
}

.password-toggle-btn:hover {
  color: #28a745;
}

.password-toggle-btn:focus {
  outline: 2px solid #28a745;
  outline-offset: 2px;
}

.form-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 1rem;
  gap: 1rem;
}

.theme-toggle {
  display: flex;
  align-items: center;
}

.theme-btn {
  background: var(--bg-secondary);
  border: 1px solid var(--border-color);
  border-radius: 0.5rem;
  padding: 0.5rem 1rem;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 0.5rem;
  transition: all 0.2s ease;
  color: var(--text-primary);
  font-size: 0.875rem;
}

.theme-btn:hover {
  background: var(--bg-tertiary);
  color: var(--accent-color);
}

@media (max-width: 480px) {
  .form-footer {
    flex-direction: column;
    gap: 0.75rem;
  }
}
</style>


<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import { useI18n } from '@/composables/useI18n.js'
import { useTheme } from '@/composables/useTheme.js'
import { signUp } from '@/lib/supabaseClient.js'

const router = useRouter()
const { t, changeLanguage, currentLanguage } = useI18n()
const { currentTheme, toggleTheme } = useTheme()

const formData = ref({
  nombre: '',
  email: '',
  password: '',
  confirmPassword: ''
})

const errors = ref({})
const isSubmitting = ref(false)
const showPassword = ref(false)
const showConfirmPassword = ref(false)

function togglePasswordVisibility() {
  showPassword.value = !showPassword.value
}

function toggleConfirmPasswordVisibility() {
  showConfirmPassword.value = !showConfirmPassword.value
}

function validateEmail(email) {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  return emailRegex.test(email)
}

function validatePassword(password) {
  return password.length >= 6
}

function validateForm() {
  errors.value = {}

  if (!formData.value.nombre.trim()) {
    errors.value.nombre = t('nameRequired')
  }

  if (!formData.value.email.trim()) {
    errors.value.email = t('emailRequired')
  } else if (!validateEmail(formData.value.email)) {
    errors.value.email = t('emailInvalid')
  }

  if (!formData.value.password) {
    errors.value.password = t('passwordRequired')
  } else if (!validatePassword(formData.value.password)) {
    errors.value.password = t('passwordMinLength')
  }

  if (!formData.value.confirmPassword) {
    errors.value.confirmPassword = t('confirmPasswordRequired')
  } else if (formData.value.password !== formData.value.confirmPassword) {
    errors.value.confirmPassword = t('passwordsDontMatch')
  }

  return Object.keys(errors.value).length === 0
}

async function handleRegister() {
  if (!validateForm()) {
    return
  }

  isSubmitting.value = true

  try {
    const { data, error } = await signUp({
      email: formData.value.email.trim().toLowerCase(),
      password: formData.value.password,
      userData: {
        nombre: formData.value.nombre.trim()
      }
    })

    if (error) {
      if (error.message.includes('ya existe')) {
        errors.value.email = t('emailAlreadyRegistered')
      } else {
        alert(t('registerError') + ': ' + error.message)
      }
      return
    }

    if (data?.user) {
      alert(t('registerSuccess'))
      formData.value = {
        nombre: '',
        email: '',
        password: '',
        confirmPassword: ''
      }
      // Redirigir al foro después del registro exitoso
      router.push('/foro')
    } else {
      alert(t('registerError'))
    }
  } catch (error) {
    console.error('Error inesperado:', error)
    alert(t('unexpectedError'))
  } finally {
    isSubmitting.value = false
  }
}
</script>
