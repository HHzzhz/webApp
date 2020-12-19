<template>
  <div>
      <van-tabbar v-model="active">
        <nuxt-link to="/h5" class="tab-item">
          <van-tabbar-item icon="home-o">Home</van-tabbar-item>
        </nuxt-link>
        <nuxt-link to="/article/search" class="tab-item">
          <van-tabbar-item icon="search">Search</van-tabbar-item>
        </nuxt-link>
        <nuxt-link to="/contactUs" class="tab-item">
          <van-tabbar-item icon="shop-collect-o">Contact Us</van-tabbar-item>
        </nuxt-link>
        <nuxt-link to="/h5/my" class="tab-item">
          <van-tabbar-item icon="contact">me</van-tabbar-item>
        </nuxt-link>
      </van-tabbar>
  </div>
</template>
