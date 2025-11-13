<script setup>
import headerContent from '@/data/headerContent.json'
</script>

<template>
  <div class="who-are-you">
    <div class="who-links">
      <NuxtLink
        v-for="(profile, index) in headerContent.whoAreYou.profiles"
        :key="index"
        :to="profile.url"
        class="who-link"
      >
        <span class="flexCS">
          <i class="icon" :class="profile.icon"></i>
          <span>
            <span class="who-title">{{ profile.title }}</span>
            <p>{{ profile.description }}</p>
          </span>
        </span>
      </NuxtLink>
      <div class="bottomLinks flexCSB">
        <a href="#">Réinitialiser</a>
        <a href="#">Je ne sais pas, voir tout</a>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.bottomLinks{
  a{
    font-weight: 500;
    font-size: 15px;
    color: $main
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
  padding: 8px;
  color: $main;
  border-radius: 8px;
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
    margin: 4px 0 0 0;
    font-size: 14px;
    color: $ink;

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
