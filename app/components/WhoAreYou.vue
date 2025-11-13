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
          <span class="who-title">{{ profile.title }}</span>
        </span>
        <p>{{ profile.description }}</p>
      </NuxtLink>
    </div>
  </div>
</template>

<style lang="scss" scoped>
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
