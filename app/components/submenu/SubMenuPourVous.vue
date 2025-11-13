<template>
  <div class="submenu-pour-vous">
    <h3>Pour vous</h3>
    <p>Contenu personnalisé selon votre profil</p>

    <!-- Exemple de structure avec liens -->
    <div class="submenu-links">
      <NuxtLink to="/particulier">Lien 1</NuxtLink>
      <NuxtLink to="/particulier">Lien 2</NuxtLink>
      <NuxtLink to="/particulier">Lien 3</NuxtLink>
    </div>

    <!-- Exemple avec image -->
    <!-- <img src="/images/example.jpg" alt=""> -->
  </div>
</template>

<style lang="scss" scoped>
.submenu-pour-vous {
  h3 {
    margin-bottom: 15px;
    color: $main;
  }

  .submenu-links {
    display: flex;
    flex-direction: column;
    gap: 10px;

    a {
      color: $ink;
      font-weight: 500;

      &:hover {
        color: $main;
      }
    }
  }
}
</style>
