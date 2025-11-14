<script setup>
import MenuList from './MenuList.vue'
import Modal from './Modal.vue'
import { onMounted, onUnmounted, ref, computed } from 'vue'
import headerContent from '@/data/headerContent.json'

// Import des composants de submenu
import SubMenuPourVous from './submenu/SubMenuPourVous.vue'
import SubMenuVegetal from './submenu/SubMenuVegetal.vue'
import SubMenuValhor from './submenu/SubMenuValhor.vue'
import SubMenuValoriser from './submenu/SubMenuValoriser.vue'
import SubMenuDurable from './submenu/SubMenuDurable.vue'
import SubMenuMarche from './submenu/SubMenuMarche.vue'
import SubMenuExcellence from './submenu/SubMenuExcellence.vue'

const headerRef = ref(null)
const currentContext = ref(null)
const activeMenuIndex = ref(0)

// Mapping des composants de submenu
const submenuComponents = [
  SubMenuPourVous,
  SubMenuVegetal,
  SubMenuValhor,
  SubMenuValoriser,
  SubMenuDurable,
  SubMenuMarche,
  SubMenuExcellence
]

// Computed pour obtenir le composant actif
const activeSubmenuComponent = computed(() => {
  return submenuComponents[activeMenuIndex.value] || null
})

// Script 1 : Sticky header au scroll + fermeture du submenu
const handleScroll = () => {
  if (!headerRef.value) return

  const headerTop = headerRef.value.querySelector('.headerTop')
  const headerBottom = headerRef.value.querySelector('.headerBottom')
  const stickyHeader = headerRef.value.querySelector('.stickyHeader')
  const headerSubMenu = headerRef.value.querySelector('.headerSubMenu')

  if (!headerTop || !headerBottom || !stickyHeader) return

  // Fermer le submenu si ouvert pendant le scroll
  if (headerSubMenu && currentContext.value !== null) {
    closeSubMenu()
  }

  const headerTopHeight = headerTop.offsetHeight
  const headerBottomHeight = headerBottom.offsetHeight

  const scrollStart = headerTopHeight + (headerBottomHeight / 2)
  const scrollEnd = headerTopHeight + headerBottomHeight

  const scrollY = window.scrollY

  if (scrollY < scrollStart) {
    stickyHeader.style.transform = 'translateY(-100%)'
  } else if (scrollY >= scrollEnd) {
    stickyHeader.style.transform = 'translateY(0)'
  } else {
    const progress = (scrollY - scrollStart) / (scrollEnd - scrollStart)
    const translateValue = -100 + (progress * 100)
    stickyHeader.style.transform = `translateY(${translateValue}%)`
  }
}

// Script 2 : Gestion du megamenu
const closeSubMenu = () => {
  if (!headerRef.value) return

  const headerSubMenu = headerRef.value.querySelector('.headerSubMenu')
  const headerBottomItems = headerRef.value.querySelectorAll('.headerBottom ul.ulWithSub li')
  const stickyHeaderItems = headerRef.value.querySelectorAll('.stickyHeader ul.ulWithSub li')

  if (headerSubMenu) {
    headerSubMenu.style.transform = 'scaleY(0)'
    headerSubMenu.style.opacity = '0'
  }

  headerBottomItems.forEach(li => li.classList.remove('active'))
  stickyHeaderItems.forEach(li => li.classList.remove('active'))

  currentContext.value = null
}

const openSubMenu = (context, index) => {
  if (!headerRef.value) return

  const headerSubMenu = headerRef.value.querySelector('.headerSubMenu')
  const headerBottom = headerRef.value.querySelector('.headerBottom')
  const stickyHeader = headerRef.value.querySelector('.stickyHeader')

  if (!headerSubMenu) return

  currentContext.value = context
  activeMenuIndex.value = index

  if (context === 'bottom') {
    const headerBottomTop = headerBottom.offsetTop
    const headerBottomHeight = headerBottom.offsetHeight

    headerSubMenu.style.position = 'absolute'
    headerSubMenu.style.top = (headerBottomTop + headerBottomHeight) + 'px'
    headerSubMenu.style.left = '0'
    headerSubMenu.style.width = '100%'
  } else if (context === 'sticky') {
    const stickyRect = stickyHeader.getBoundingClientRect()

    headerSubMenu.style.position = 'fixed'
    headerSubMenu.style.top = (stickyRect.bottom) + 'px'
    headerSubMenu.style.left = '0'
    headerSubMenu.style.width = '100%'
  }

  headerSubMenu.style.transform = 'scaleY(1)'
  headerSubMenu.style.opacity = '1'
}

const handleMenuItemClick = (context, index, event) => {
  const clickedItem = event.currentTarget
  const wasActive = clickedItem.classList.contains('active')
  
  if (!headerRef.value) return
  
  const headerBottomItems = headerRef.value.querySelectorAll('.headerBottom ul.ulWithSub li')
  const stickyHeaderItems = headerRef.value.querySelectorAll('.stickyHeader ul.ulWithSub li')
  
  headerBottomItems.forEach(li => li.classList.remove('active'))
  stickyHeaderItems.forEach(li => li.classList.remove('active'))
  
  if (!wasActive) {
    clickedItem.classList.add('active')
    openSubMenu(context, index)
  } else {
    closeSubMenu()
  }
}

const handleTopMenuClick = () => {
  closeSubMenu()
}

// Script 3 : Gestion Modal (WhoAreYou et SearchHeader)
const isModalOpen = ref(false)
const modalType = ref('who') // 'who' ou 'search'

const getModalTitle = () => {
  return modalType.value === 'who'
    ? headerContent.whoAreYou.title
    : headerContent.search.title
}

const handleWhoClick = (event) => {
  event.preventDefault()
  modalType.value = 'who'
  isModalOpen.value = true
}

const handleSearchClick = (event) => {
  event.preventDefault()
  modalType.value = 'search'
  isModalOpen.value = true
}

const closeModal = () => {
  isModalOpen.value = false
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll)
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
})
</script>

<template>
  <header class="header" ref="headerRef">
    <div class="headerTop">
      <div class="container">
        <div class="flexCSB">
          <NuxtLink to="/" class="logo">
            <img src="/images/logo-white.svg" alt="">
          </NuxtLink>
          <ul class="menu" @click="handleTopMenuClick">
            <li v-for="(item, index) in headerContent.topMenu" :key="index">
              <NuxtLink :to="item.url">{{ item.label }}</NuxtLink>
            </li>
          </ul>
    <div class="headerActionDesktop">
      <NuxtLink to="/" class="btn outlineWhite espacePro"><span>{{ headerContent.actions.espacePro }}</span><i class="icon icon-arrow-right"></i></NuxtLink>
      <a href="#" class="btn outlineWhite whoTrigger" @click="handleWhoClick"><span>{{ headerContent.actions.youAre }}</span><i class="icon icon-priority-low"></i></a>
      <a href="#" class="btn outlineWhite searchTrigger" @click="handleSearchClick"><i class="icon icon-magnifier"></i></a>
    </div>
    
    <!-- Mobile aussi -->
    <div class="headerActionMobile">
      <NuxtLink to="/" class="btnFull espacePro"><i class="icon icon-suitcase"></i></NuxtLink>
      <a href="#" class="whoTrigger btnFull" @click="handleWhoClick"><i class="icon icon-user"></i></a>
      <a href="#" class="searchTrigger btnFull" @click="handleSearchClick"><i class="icon icon-magnifier"></i></a>
      <NuxtLink to="/" class="btnFull"><i class="icon icon-menu"></i></NuxtLink>
    </div>
        </div>
      </div>
    </div>
    
    <div class="headerBottom">
      <div class="container">
        <MenuList context="bottom" @menuClick="handleMenuItemClick" />
      </div>
    </div>
    
    <div class="stickyHeader">
      <div class="container">
        <div class="flexCSB">
          <NuxtLink to="/" class="logo">
            <img src="/images/logo-green-scroll.svg" alt="">
          </NuxtLink>
          <MenuList context="sticky" @menuClick="handleMenuItemClick" />
          
          <div class="headerActionDesktop">
            <NuxtLink to="/" class="btn outlineMain small espacePro"><span>{{ headerContent.actions.espacePro }}</span><i class="icon icon-arrow-right"></i></NuxtLink>
            <NuxtLink to="/" class="btn outlineMain small whoTrigger" @click="handleWhoClick"><span>{{ headerContent.actions.youAre }}</span><i class="icon icon-priority-low"></i></NuxtLink>
            <NuxtLink to="/" class="btn outlineMain small searchTrigger" @click="handleSearchClick"><i class="icon icon-magnifier"></i></NuxtLink>
          </div>
        </div>
      </div>
    </div>
    
    <div class="headerSubMenu">
      <div class="container">
        <component
          :is="activeSubmenuComponent"
          v-if="activeSubmenuComponent"
          class="subMenuContent"
        />
      </div>
    </div>

  </header>

  <!-- Modal Component -->
  <Modal
    :is-open="isModalOpen"
    :type="modalType"
    :title="getModalTitle()"
    @close="closeModal"
  />
</template>


<style lang="scss" scoped>
.headerTop {
  height: 76px;
  background-color: $main;
  display: flex;
  align-items: center;
  transition: $trans;
  .logo {
    img {
      height: 44px;
    }
  }
  .espacePro {
    i.icon.icon-arrow-right {
      transform: rotate(-45deg);
    }
  }
}

.headerBottom {
  background-color: $white;
  transition: $trans;

  display: flex;
  align-items: center;
}

.headerBottom {
  :deep(ul) {
    max-width: 1100px;
    li{
      padding: 0 20px;
      span {
        font-size: 16px;
        height: 76px;
      }
      &:first-child{
        background-color: $light;
        span{
          font-weight: 600;
          color: $main;
          text-transform: uppercase;
        }
      }
    }
  }
}

.stickyHeader {
  :deep(ul) {
    max-width: 100%;
    li{
        padding: 0 20px;
        margin: 0 0px;
        span{
          height: 56px;

        }
      span {
      font-size: 15px;
    }
    }
  }
}

.headerSubMenu {
  background-color: $light;
  display: flex;
  align-items: center;
  opacity: 0;
  transform: scaleY(0);
  transform-origin: top;
  transition: transform 0.3s ease, opacity 0.3s ease;
  position: fixed; // Ajouté pour le positionnement dynamique
  z-index: 5; // En dessous du sticky mais au-dessus du contenu
  width: 100%;
  border-bottom: 1px solid rgba($main, .1);
  .container{
    padding: 20px 0;
  }

  p {
    margin: 0 !important;
  }
}

.stickyHeader {
  position: fixed;
  top: 0px;
  left: 0;
  width: 100%;
  background-color: #fff;
  z-index: 10;
  transition: $trans;
  transform: translateY(-100%);
  .logo {
    img {
      height: 30px;
    }

  }

  ul {
    display: flex;
    margin: 0;

    li {
      padding: 0 15px;
      text-align: center;
      height: 50px;
      display: flex;
      align-items: center;
      transition: $trans;
      &.active {
        background-color: $light;
      }

      span {
        font-size: 14px;
        font-weight: 500;
      }
    }
  }

  .headerActionDesktop {
    .btn {
      color: $main;
    }
  }
}

header.show-sticky .stickyHeader {
  transform: none;
}

.header {
  box-shadow: 0 1rem 3rem rgba(0, 0, 0, 0.175);
  li{
    cursor: pointer;
  }

}

ul {
  &.menu {
    display: flex;
    align-items: center;
    justify-content: start;
    transition: $trans;

    li {
      margin: 0 20px;

      a {
        font-weight: 500;
        font-size: 16px;
        display: block;
        padding: 4px 0;
        border-top: 2px solid transparent;
        border-bottom: 2px solid transparent;
        transition: $trans;
        color: $white;

        &:hover {
          border-bottom-color: $white
        }
      }
    }
  }
}

.headerActionMobile {
  display: none;
}

.headerActionDesktop {
  display: flex;

  .btn {
    margin-left: 20px;
  }
}

@media screen and (max-width: 1199px) {
  .headerActionMobile {
    display: flex;
  }

  .headerActionDesktop {
    display: none;
  }

  ul.menu {
    display: none;
  }

  .headerTop {
    height: 54px;

    .logo {
      img {
        height: 32px;
      }
    }
  }

  .headerBottom {
    display: none;
  }
}
</style>