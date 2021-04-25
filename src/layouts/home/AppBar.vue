<template>
  <div>
    <v-app-bar
      id="home-app-bar"
      elevation="1"
      elevate-on-scroll
      app
      height="80"
    >
      <base-img
        :src="require('@/assets/logo.png')"
        class="ml-8"
        contain
        max-width="70"
        width="100%"
      />
      <v-spacer />

      <div>
        <v-tabs
          optional
          background-color="transparent"
        >
          <v-tab
            v-for="(item, i) in items"
            :key="i"
            :to="item.route"
            :ripple="false"
            class="font-weight-bold"
            min-width="96"
            text
          >
            {{ item.title }}
          </v-tab>
        </v-tabs>
      </div>
    </v-app-bar>
  </div>
</template>

<script>
  export default {
    name: 'HomeAppBar',

    data: () => ({
      items: [
        { title: 'حجز الكورسات', route: 'course-reservation' },
        { title: 'الرئيسية', route: '/' },
      ],
    }),
  }
</script>

<style lang="sass">
#home-app-bar
  .v-tabs-slider
    max-width: 24px
    margin: 0 auto

  .v-tab
    &::before
      display: none
</style>
