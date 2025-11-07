<template>
  <header>
    <nav>
      <ul>
        <li><NuxtLink to="/index">HOME</NuxtLink></li>
        <li><NuxtLink to="/actualites">Actualités</NuxtLink></li>
        <li><NuxtLink to="/qui-sommes-nous">Qui sommes-nous</NuxtLink></li>
        <li><NuxtLink to="/notre-histoire">Notre histoire</NuxtLink></li>
      </ul>
    </nav>
  </header>
</template>


<style lang="scss" scoped>
header {
  nav {
    ul {
      list-style: none;
      display: flex;
      gap: 20px;
      padding: 0;

      li {
        a {
          color: #333;
          text-decoration: none;
          padding: 10px 15px;
          transition: color 0.3s ease;

          &:hover {
            color: #007bff;
          }
        }
      }
    }
  }
}
</style>