<template>
  <div class="who-are-you">
    <h2>Vous êtes ?</h2>
    <div class="who-links">
      <NuxtLink to="/particulier" class="who-link">
        <span class="flexCS">
          <i class="icon icon-user"></i>
          <span class="who-title">Particulier</span>
        </span>
        <p>Idées, conseil, trouver un pro</p>
      </NuxtLink>

      <NuxtLink to="/professionnel" class="who-link">
        <span class="flexCS">
          <i class="icon icon-suitcase"></i>
          <span class="who-title">Professionnel</span>
        </span>
        <p>Idées, conseil, trouver un pro</p>
      </NuxtLink>

      <NuxtLink to="/etudiant" class="who-link">
        <span class="flexCS">
          <i class="icon icon-student"></i>
          <span class="who-title">Étudiant/Jeune</span>
        </span>
        <p>Idées, conseil, trouver un pro</p>
      </NuxtLink>

      <NuxtLink to="/presse" class="who-link">
        <span class="flexCS">
          <i class="icon icon-bullhorn"></i>
          <span class="who-title">Presse/partenaires</span>
        </span>
        <p>Idées, conseil, trouver un pro</p>
      </NuxtLink>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.who-are-you {
  h2 {
    margin: 0 0 20px 0;
    font-size: 24px;
    color: $main;
    text-align: center;
  }
}

.who-links {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.who-link {
  background-color: white;
  display: block;
  padding: 15px;
  color: $main;
  border-radius: 10px;
  border: 2px solid $light;
  transition: $trans;
  text-decoration: none;

  * {
    transition: inherit;
  }

  i {
    font-size: 20px;
    margin-right: 10px;
    color: $main;
  }

  .who-title {
    font-weight: 500;
    font-size: 16px;
    color: $main;
  }

  p {
    margin: 8px 0 0 0;
    font-size: 14px;
    color: $ink;
    padding-left: 30px;
  }

  &:hover {
    background-color: $main;
    border-color: $main;

    * {
      color: white !important;
    }
  }
}
</style>
