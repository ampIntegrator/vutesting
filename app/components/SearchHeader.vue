<script setup>
import { ref } from 'vue'
import headerContent from '@/data/headerContent.json'

const searchQuery = ref('')

const handleSearch = () => {
  if (searchQuery.value.trim()) {
    // TODO: Implémenter la logique de recherche
    console.log('Recherche:', searchQuery.value)
    // Naviguer vers la page de résultats ou appeler une API
  }
}
</script>

<template>
  <div class="search-header">
    <form @submit.prevent="handleSearch">
      <div class="search-input-group">
        <input
          type="text"
          v-model="searchQuery"
          :placeholder="headerContent.search.placeholder"
          class="search-input"
        />
        <button type="submit" class="btn fillMain">
          <i class="icon icon-magnifier"></i>
        </button>
      </div>
    </form>
  </div>
</template>

<style lang="scss" scoped>
.search-input-group {
  display: flex;
  align-items: stretch;
}

.search-input {
  flex-grow: 1;
  height: 36px;
  display: block;
    border-top-left-radius: 6px;
    border-bottom-left-radius: 6px;
  padding: 0 8px;
  outline: none;
  font-size: 15px;
  transition: $trans;
  border: 0;
  background-color: $light;

  &::placeholder {
    color: #999;
  }
}

.btn {
  height: 36px;
  min-width: 36px;
    border-top-left-radius: 0px;
    border-bottom-left-radius: 0px;
    border-top-right-radius: 6px;
    border-bottom-right-radius: 6px;
  display: flex;
  align-items: center;
  justify-content: center;

  i {
    font-size: 20px;
  }
}
</style>
