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
        <button type="submit" class="btn btnOutline search-btn">
          <i class="icon icon-magnifier"></i>
        </button>
      </div>
    </form>
  </div>
</template>

<style lang="scss" scoped>
.search-input-group {
  display: flex;
  gap: 10px;
  align-items: stretch;
}

.search-input {
  flex-grow: 1;
  height: 44px;
  display: block;
  border-radius: 6px;
  border: 2px solid $light;
  padding: 0 15px;
  outline: none;
  font-size: 16px;
  transition: $trans;

  &:focus {
    border-color: $main;
    box-shadow: 0 0 0 3px rgba($main, 0.1);
  }

  &::placeholder {
    color: #999;
  }
}

.search-btn {
  height: 44px;
  min-width: 44px;
  padding: 0 15px;
  display: flex;
  align-items: center;
  justify-content: center;

  i {
    font-size: 20px;
  }
}
</style>
