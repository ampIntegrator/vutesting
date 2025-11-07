<template>
  <header :class="{ 'scrolled': isScrolled }">
    <!-- Logo change au scroll -->
    <div class="logo-container">
      <img
        v-if="!isScrolled"
        src="/images/logos/logo-green.svg"
        alt="Logo Vert"
        class="logo"
      />
      <img
        v-else
        src="/images/logos/logo-green-scroll.svg"
        alt="Logo Vert Scroll"
        class="logo"
      />
    </div>

    <nav>
      <ul>
        <li><NuxtLink to="/">HOME</NuxtLink></li>
        <li><NuxtLink to="/actualites">Actualités</NuxtLink></li>
        <li><NuxtLink to="/qui-sommes-nous">Qui sommes-nous</NuxtLink></li>
        <li><NuxtLink to="/notre-histoire">Notre histoire</NuxtLink></li>
      </ul>
    </nav>
  </header>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const isScrolled = ref(false)

const handleScroll = () => {
  isScrolled.value = window.scrollY > 100
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll)
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
})
</script>

<style lang="scss" scoped>
header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 20px 40px;
  background: white;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  z-index: 1000;
  transition: all 0.3s ease;

  &.scrolled {
    padding: 10px 40px;
    background: rgba(255, 255, 255, 0.95);
  }

  .logo-container {
    .logo {
      height: 50px;
      width: auto;
      transition: height 0.3s ease;
    }
  }

  &.scrolled .logo {
    height: 40px;
  }

  nav {
    ul {
      list-style: none;
      display: flex;
      gap: 20px;
      padding: 0;
      margin: 0;

      li {
        a {
          color: #333;
          text-decoration: none;
          padding: 10px 15px;
          transition: color 0.3s ease;
          font-weight: 500;

          &:hover {
            color: #007bff;
          }

          &.router-link-active {
            color: #007bff;
            font-weight: 600;
          }
        }
      }
    }
  }
}
</style>
