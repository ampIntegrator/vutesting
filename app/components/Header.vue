<script setup>
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
            <li><NuxtLink to="/">Cotisations & avantages</NuxtLink></li>
            <li><NuxtLink to="/notre-histoire">Cité verte</NuxtLink></li>
            <li><NuxtLink to="/qui-sommes-nous">Le Végétal, c'est la vie</NuxtLink></li>
          </ul>
          <div class="headerActionDesktop">
            <NuxtLink to="/" class="btn outlineWhite espacePro"><span>Espace Pro</span><i class="icon icon-arrow-right"></i></NuxtLink>
            <NuxtLink to="/" class="btn outlineWhite whoTrigger"><span>Vous êtes ?</span><i class="icon icon-priority-low"></i></NuxtLink>
            <NuxtLink to="/" class="btn outlineWhite searchTrigger"><i class="icon icon-magnifier"></i></NuxtLink>
          </div>
          <div class="headerActionMobile">
            <NuxtLink to="/" class="btnFull espacePro"><i class="icon icon-suitcase"></i></NuxtLink>
            <NuxtLink to="/" class="whoTrigger btnFull"><i class="icon icon-user"></i></NuxtLink>
            <NuxtLink to="/" class="searchTrigger btnFull"><i class="icon icon-magnifier"></i></NuxtLink>
            <NuxtLink to="/" class="searchTrigger btnFull"><i class="icon icon-menu"></i></NuxtLink>
          </div>
        </div>
      </div>
    </div>
    
    <div class="headerBottom">
      <div class="container">
        <ul class="ulWithSub">
          <li @click="handleMenuItemClick('bottom', 0, $event)"><span>Le Végétal &<br>les métiers du futur</span></li>
          <li @click="handleMenuItemClick('bottom', 1, $event)"><span>L'interprofession<br>VALHOR</span></li>
          <li @click="handleMenuItemClick('bottom', 2, $event)"><span>Valoriser<br>la filière</span></li>
          <li @click="handleMenuItemClick('bottom', 3, $event)"><span>Filière<br>durable</span></li>
          <li @click="handleMenuItemClick('bottom', 4, $event)"><span>Le marché<br>du Végétal</span></li>
          <li @click="handleMenuItemClick('bottom', 5, $event)"><span>Faire pousser<br>l'excellence</span></li>
        </ul>
      </div>
    </div>
    
    <div class="stickyHeader">
      <div class="container">
        <div class="flexCSB">
          <NuxtLink to="/" class="logo">
            <img src="/images/logo-green-scroll.svg" alt="">
          </NuxtLink>
          <ul>
            <li @click="handleMenuItemClick('sticky', 0, $event)"><span>Le Végétal &<br>les métiers du futur</span></li>
            <li @click="handleMenuItemClick('sticky', 1, $event)"><span>L'interprofession<br>VALHOR</span></li>
            <li @click="handleMenuItemClick('sticky', 2, $event)"><span>Valoriser<br>la filière</span></li>
            <li @click="handleMenuItemClick('sticky', 3, $event)"><span>Filière<br>durable</span></li>
            <li @click="handleMenuItemClick('sticky', 4, $event)"><span>Le marché<br>du Végétal</span></li>
            <li @click="handleMenuItemClick('sticky', 5, $event)"><span>Faire pousser<br>l'excellence</span></li>
          </ul>
          
          <div class="headerActionDesktop">
            <NuxtLink to="/" class="btn outlineMain small espacePro"><span>Espace Pro</span><i class="icon icon-arrow-right"></i></NuxtLink>
            <NuxtLink to="/" class="btn outlineMain small whoTrigger"><span>Vous êtes ?</span><i class="icon icon-priority-low"></i></NuxtLink>
            <NuxtLink to="/" class="btn outlineMain small searchTrigger"><i class="icon icon-magnifier"></i></NuxtLink>
          </div>
        </div>
      </div>
    </div>
    
    <div class="headerSubMenu">
      <div class="container">
        <!-- Contenu 1 -->
        <div class="subMenuContent" v-show="activeMenuIndex === 0">
          <h3>Le Végétal & les métiers du futur</h3>
          <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Découvrez les métiers passionnants du végétal...</p>
        </div>
        
        <!-- Contenu 2 -->
        <div class="subMenuContent" v-show="activeMenuIndex === 1">
          <h3>L'interprofession VALHOR</h3>
          <p>VALHOR représente l'ensemble des professionnels de l'horticulture et du paysage...</p>
        </div>
        
        <!-- Contenu 3 -->
        <div class="subMenuContent" v-show="activeMenuIndex === 2">
          <h3>Valoriser la filière</h3>
          <p>Nos actions pour promouvoir et développer la filière du végétal en France...</p>
        </div>
        
        <!-- Contenu 4 -->
        <div class="subMenuContent" v-show="activeMenuIndex === 3">
          <h3>Filière durable</h3>
          <p>Engagement environnemental et développement durable dans nos pratiques...</p>
        </div>
        
        <!-- Contenu 5 -->
        <div class="subMenuContent" v-show="activeMenuIndex === 4">
          <h3>Le marché du Végétal</h3>
          <p>Chiffres clés, tendances et perspectives du marché horticole français...</p>
        </div>
        
        <!-- Contenu 6 -->
        <div class="subMenuContent" v-show="activeMenuIndex === 5">
          <h3>Faire pousser l'excellence</h3>
          <p>Formation, innovation et excellence au service des professionnels du végétal...</p>
        </div>
      </div>
    </div>
  </header>
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

  .container {
    ul {
      max-width: 1100px;
      margin: 0 auto;
      display: flex;
      align-items: center;
      transition: $trans;

      li {
        flex-grow: 1;
        height: 76px;
        display: flex;
        align-items: center;
        justify-content: start;
        transition: $trans;
        text-align: center;

        span {
          display: block;
          width: 100%;
          color: $ink;
          font-weight: 500;
        }

        &.active {
          background-color: $light;

          span {
            color: $main
          }
        }
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