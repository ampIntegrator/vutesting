<script setup>
import Compotest from '~/components/Compotest.vue'
</script>

<template>
  <div class="container">
    <h1>Actualités</h1>

    <Compotest 
      titre="Mon titre personnalisé"
      texte="Mon texte personnalisé pour cette page"
      className="ma-classe-custom"
    />

    <p>Lorem ipsum dolor sit amet...</p>
    
    <!-- Tu peux l'appeler plusieurs fois avec des contenus différents -->
    <Compotest 
      titre="Un autre titre"
      texte="Un autre texte complètement différent"
      className="autre-classe"
    />
  </div>
</template>