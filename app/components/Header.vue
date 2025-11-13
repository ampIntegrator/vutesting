<script setup>
import MenuList from './MenuList.vue'
import { onMounted, onUnmounted, ref } from 'vue'

const headerRef = ref(null)
const currentContext = ref(null)
const activeMenuIndex = ref(0)

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
    headerSubMenu.style.height = '0px'
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
  
  headerSubMenu.style.height = '200px'
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

// Script 3 : Gestion whoAreYou et searchHeader
const closeAll = () => {
  const whoAreYou = document.getElementById('whoAreYou')
  const searchHeader = document.getElementById('searchHeader')
  const backdrop = document.querySelector('.backdrop')
  
  if (!whoAreYou || !backdrop) return
  
  const isMobile = window.innerWidth <= 1199
  
  backdrop.style.opacity = '0'
  
  if (whoAreYou.classList.contains('is-open')) {
    whoAreYou.style.opacity = '0'
    if (isMobile) {
      whoAreYou.style.transform = 'translateX(-50%) scaleY(0)'
    } else {
      whoAreYou.style.transform = 'scaleY(0)'
    }
  }
  
  if (searchHeader && searchHeader.classList.contains('is-open')) {
    searchHeader.style.opacity = '0'
    searchHeader.style.transform = 'scaleY(0)'
  }
  
  setTimeout(() => {
    whoAreYou.classList.remove('is-open')
    if (searchHeader) searchHeader.classList.remove('is-open')
    backdrop.classList.remove('is-open')
  }, 300)
}

const openWhoAreYou = (event) => {
  const trigger = event.currentTarget
  const whoAreYou = document.getElementById('whoAreYou')
  const backdrop = document.querySelector('.backdrop')
  
  if (!whoAreYou || !backdrop) return
  
  backdrop.classList.add('is-open')
  backdrop.style.opacity = '1'
  
  const isMobile = window.innerWidth <= 1199
  
  if (isMobile) {
    whoAreYou.style.top = '20px'
    whoAreYou.style.left = '50%'
    whoAreYou.style.right = 'auto'
    whoAreYou.style.transform = 'translateX(-50%) scaleY(0)'
    whoAreYou.style.maxWidth = '300px'
    whoAreYou.style.width = 'calc(100% - 40px)'
  } else {
    whoAreYou.style.transform = 'scaleY(0)'
    whoAreYou.style.maxWidth = ''
    whoAreYou.style.width = '300px'
    whoAreYou.style.right = 'auto'
    
    const triggerRect = trigger.getBoundingClientRect()
    
    whoAreYou.style.top = (triggerRect.top) + 'px'
    whoAreYou.style.left = (triggerRect.right - 300) + 'px'
  }
  
  whoAreYou.classList.add('is-open')
  whoAreYou.style.opacity = '0'
  whoAreYou.offsetHeight
  
  setTimeout(() => {
    whoAreYou.style.opacity = '1'
    if (isMobile) {
      whoAreYou.style.transform = 'translateX(-50%) scaleY(1)'
    } else {
      whoAreYou.style.transform = 'scaleY(1)'
    }
  }, 10)
}

const openSearchHeader = () => {
  const searchHeader = document.getElementById('searchHeader')
  const backdrop = document.querySelector('.backdrop')
  
  if (!searchHeader || !backdrop) return
  
  backdrop.classList.add('is-open')
  backdrop.style.opacity = '1'
  
  searchHeader.style.opacity = '0'
  searchHeader.style.transform = 'scaleY(0)'
  searchHeader.classList.add('is-open')
  searchHeader.offsetHeight
  
  setTimeout(() => {
    searchHeader.style.opacity = '1'
    searchHeader.style.transform = 'scaleY(1)'
  }, 10)
}

const handleWhoClick = (event) => {
  event.preventDefault()
  const whoAreYou = document.getElementById('whoAreYou')
  const searchHeader = document.getElementById('searchHeader')
  
  const isVisible = whoAreYou?.classList.contains('is-open')
  
  if (!isVisible) {
    if (searchHeader?.classList.contains('is-open')) {
      closeAll()
      setTimeout(() => openWhoAreYou(event), 300)
    } else {
      openWhoAreYou(event)
    }
  } else {
    closeAll()
  }
}

const handleSearchClick = (event) => {
  event.preventDefault()
  const whoAreYou = document.getElementById('whoAreYou')
  const searchHeader = document.getElementById('searchHeader')
  
  const isVisible = searchHeader?.classList.contains('is-open')
  
  if (!isVisible) {
    if (whoAreYou?.classList.contains('is-open')) {
      closeAll()
      setTimeout(openSearchHeader, 300)
    } else {
      openSearchHeader()
    }
  } else {
    closeAll()
  }
}

const handleBackdropClick = () => {
  closeAll()
}

const handleDocumentClick = (e) => {
  const whoAreYou = document.getElementById('whoAreYou')
  const searchHeader = document.getElementById('searchHeader')
  const backdrop = document.querySelector('.backdrop')
  
  const whoTriggers = document.querySelectorAll('.whoTrigger')
  const searchTriggers = document.querySelectorAll('.searchTrigger')
  
  const isWhoTrigger = Array.from(whoTriggers).some(trigger => trigger.contains(e.target))
  const isSearchTrigger = Array.from(searchTriggers).some(trigger => trigger.contains(e.target))
  
  if (!isWhoTrigger && 
      !whoAreYou?.contains(e.target) && 
      !isSearchTrigger && 
      !searchHeader?.contains(e.target) && 
      !backdrop?.contains(e.target)) {
    closeAll()
  }
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll)
  document.addEventListener('click', handleDocumentClick)
  
  // Ajouter les listeners aux triggers
  const backdrop = document.querySelector('.backdrop')
  if (backdrop) {
    backdrop.addEventListener('click', handleBackdropClick)
  }
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
  document.removeEventListener('click', handleDocumentClick)
  
  const backdrop = document.querySelector('.backdrop')
  if (backdrop) {
    backdrop.removeEventListener('click', handleBackdropClick)
  }
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
            <li><NuxtLink to="/actualites">Cotisations & avantages</NuxtLink></li>
            <li><NuxtLink to="/notre-histoire">Cité verte</NuxtLink></li>
            <li><NuxtLink to="/qui-sommes-nous">Le Végétal, c'est la vie</NuxtLink></li>
          </ul>
    <div class="headerActionDesktop">
      <NuxtLink to="/" class="btn outlineWhite espacePro"><span>Espace Pro</span><i class="icon icon-arrow-right"></i></NuxtLink>
      <a href="#" class="btn outlineWhite whoTrigger" @click="handleWhoClick"><span>Vous êtes ?</span><i class="icon icon-priority-low"></i></a>
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
            <NuxtLink to="/" class="btn outlineMain small espacePro"><span>Espace Pro</span><i class="icon icon-arrow-right"></i></NuxtLink>
            <NuxtLink to="/" class="btn outlineMain small whoTrigger" @click="handleWhoClick"><span>Vous êtes ?</span><i class="icon icon-priority-low"></i></NuxtLink>
            <NuxtLink to="/" class="btn outlineMain small searchTrigger"><i class="icon icon-magnifier"></i></NuxtLink>
          </div>
        </div>
      </div>
    </div>
    
    <div class="headerSubMenu">
      <div class="container">
        <!-- NOUVEAU Contenu 1 pour "Recommandé pour vous" -->
        <div class="subMenuContent" v-show="activeMenuIndex === 0">
          <h3>Recommandé pour vous</h3>
          <p>Ici, on trouvera les liens liés à chaque profil</p>
        </div>
        
        <!-- Contenu 2 (ancien 1) -->
        <div class="subMenuContent" v-show="activeMenuIndex === 1">
          <h3>Le Végétal & les métiers du futur</h3>
          <p>Lorem ipsum dolor sit amet...</p>
        </div>
        
        <!-- Contenu 3 (ancien 2) -->
        <div class="subMenuContent" v-show="activeMenuIndex === 2">
          <h3>L'interprofession VALHOR</h3>
          <p>VALHOR représente...</p>
        </div>
        
        <!-- Contenu 4 (ancien 3) -->
        <div class="subMenuContent" v-show="activeMenuIndex === 3">
          <h3>Valoriser la filière</h3>
          <p>Nos actions pour promouvoir...</p>
        </div>
        
        <!-- Contenu 5 (ancien 4) -->
        <div class="subMenuContent" v-show="activeMenuIndex === 4">
          <h3>Filière durable</h3>
          <p>Engagement environnemental...</p>
        </div>
        
        <!-- Contenu 6 (ancien 5) -->
        <div class="subMenuContent" v-show="activeMenuIndex === 5">
          <h3>Le marché du Végétal</h3>
          <p>Chiffres clés, tendances...</p>
        </div>
        
        <!-- Contenu 7 (ancien 6) -->
        <div class="subMenuContent" v-show="activeMenuIndex === 6">
          <h3>Faire pousser l'excellence</h3>
          <p>Formation, innovation...</p>
        </div>
      </div>
    </div>

  </header>
  <div id="whoAreYou">
  <div class="innerWho">
      <a href="#">
          <span class="flexCS">
              <i class="icon icon-user"></i>
              <span>Particulier</span>
          </span>
          <p>Idées, conseil, trouver un pro</p>
      </a>
      <a href="#">
          <span class="flexCS">
              <i class="icon icon-suitcase"></i>
              <span>Professionnel</span>
          </span>
          <p>Idées, conseil, trouver un pro</p>
      </a>
      <a href="#">
          <span class="flexCS">
              <i class="icon icon-student"></i>
              <span>Étudiant/Jeune</span>
          </span>
          <p>Idées, conseil, trouver un pro</p>
      </a>
      <a href="#">
          <span class="flexCS">
              <i class="icon icon-bullhorn"></i>
              <span>Presse/partenaires</span>
          </span>
          <p>Idées, conseil, trouver un pro</p>
      </a>
      <a href="#">
          <span class="flexCS">
              <i class="icon icon-greek-temple"></i>
              <span>Institution/Collectivité</span>
          </span>
          <p>Idées, conseil, trouver un pro</p>
      </a>
      <a href="#">
          <span class="flexCS">
              <i class="icon icon-alert-question"></i>
              <span>Je ne sais pas</span>
          </span>
      </a>

  </div>
</div>
<div class="backdrop"></div>
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
          height: 50px;

        }
      span {
      font-size: 14px;
    }
    }
  }
}

.headerSubMenu {
  background-color: $light;
  height: 0;
  overflow: hidden;
  display: flex;
  align-items: center;
  opacity: 0;
  transition: height 0.3s ease, opacity 0.3s ease;
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
  height: 50px;
  .logo {
    img {
      height: 26px;
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

#whoAreYou {
    opacity: 0;
    transform: scaleY(0);
    transform-origin: center center;
    visibility: hidden;
    transition: opacity 0.3s ease, transform 0.3s ease;
}

#whoAreYou.is-open {
    opacity: 1;
    transform: scaleY(1);
    visibility: visible;
}

#whoAreYou {
    position: fixed;
    width: 300px;
    z-index: 40;
    a {
        background-color: white;
        ;
        display: block;
        padding: 10px 15px;
        color: $main;
        border-radius: 10px;
        margin-bottom: 10px;
        transition: $trans;

        * {
            transition: inherit;
        }

        i {
            font-size: 20px;
            margin-right: 8px;
        }

        >span {

            span {
                font-weight: 500;
                font-size: 15px;
            }
        }

        p {
            margin: 6px 0 0 0;
            font-size: 14px;
        }

        &:hover {
            background-color: $main;

            * {
                color: white !important
            }
        }
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
.backdrop {
  position: fixed;
  z-index: 20;
  width: 100%;
  height: 100vh;
  top: 0;
  left: 0;
  background-color: rgba(0, 0, 0, 0.75);
  /* Ajuste selon ton $main */
  opacity: 0;
  visibility: hidden;
  -webkit-transition: opacity 0.3s ease;
  transition: opacity 0.3s ease;
}

.backdrop.is-open {
  opacity: 1;
  visibility: visible;
}
</style>