<template>
  <Transition name="modal">
    <div v-if="isOpen" class="modal-backdrop" @click="handleBackdropClick">
      <div class="modal" @click.stop>
        <div class="modal-header">
          <h3 class="modal-title">{{ title }}</h3>
          <button class="modal-close" @click="closeModal" aria-label="Fermer">
            <i class="icon icon-xmark"></i>
          </button>
        </div>
        <div class="modal-content">
          <WhoAreYou v-if="type === 'who'" />
          <SearchHeader v-if="type === 'search'" />
        </div>
      </div>
    </div>
  </Transition>
</template>

<script setup>
import { watch } from 'vue'
import WhoAreYou from './WhoAreYou.vue'
import SearchHeader from './SearchHeader.vue'

const props = defineProps({
  isOpen: {
    type: Boolean,
    required: true
  },
  type: {
    type: String,
    validator: (value) => ['who', 'search'].includes(value)
  },
  title: {
    type: String,
    required: true
  }
})

const emit = defineEmits(['close'])

const closeModal = () => {
  emit('close')
}

const handleBackdropClick = () => {
  closeModal()
}

// Gestion overflow-hidden sur body quand modal ouverte
watch(() => props.isOpen, (newValue) => {
  // Vérifier que nous sommes côté client (pas SSR)
  if (typeof document !== 'undefined') {
    if (newValue) {
      document.body.style.overflow = 'hidden'
    } else {
      document.body.style.overflow = ''
    }
  }
}, { immediate: true })
</script>

<style lang="scss" scoped>
.modal-backdrop {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.75);
  z-index: 100;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal {
  position: relative;
  background-color: white;
  border-radius: 10px;
  max-width: 90%;
  width: 500px;
  max-height: 90vh;
  overflow-y: auto;
  padding: 0;
  transform-origin: center;
}

.modal-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 20px 30px;
  border-bottom: 1px solid $light;
}

.modal-title {
  margin: 0;
  font-size: 24px;
  color: $main;
  font-weight: 600;
}

.modal-close {
  background: none;
  border: none;
  cursor: pointer;
  font-size: 24px;
  color: $ink;
  transition: color 0.3s ease;
  padding: 5px;
  line-height: 1;
  flex-shrink: 0;

  &:hover {
    color: $main;
  }

  i {
    display: block;
  }
}

.modal-content {
  padding: 30px;
}

// Transitions Vue
.modal-enter-active,
.modal-leave-active {
  transition: opacity 0.3s ease;

  .modal {
    transition: transform 0.3s ease;
  }
}

.modal-enter-from {
  opacity: 0;

  .modal {
    transform: scaleY(0);
  }
}

.modal-leave-to {
  opacity: 0;

  .modal {
    transform: scaleY(0);
  }
}
</style>
