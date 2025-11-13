<script setup>
const props = defineProps({
  titre: {
    type: String,
    required: true
  },
  texte: {
    type: String,
    required: true
  },
  className: {
    type: String,
    default: ''
  }
})
</script>

<template>
  <div class="compotest" :class="className">
    <h3>{{ titre }}</h3>
    <p>{{ texte }}</p>
  </div>
</template>

<style lang="scss" scoped>
.compotest {
  padding: 20px;
  background-color: $light;

}
</style>