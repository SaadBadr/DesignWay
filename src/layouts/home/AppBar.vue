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
        class="ml-4"
        contain
        max-width="70"
        width="100%"
      />
      <v-spacer />

      <!-- Burger navigation menu for sm devices -->

      <template v-if="$vuetify.breakpoint.smAndDown">
        <v-menu offset-y>
          <template v-slot:activator="{ on, attrs }">
            <v-app-bar-nav-icon
              v-bind="attrs"
              v-on="on"
            />
          </template>
          <v-list
            nav
            dense
          >
            <v-list-item-group
              v-model="group"
              active-class="deep-purple--text text--accent-4"
            >
              <v-list-item
                v-for="(item, i) in items.slice().reverse()"
                :key="i"
                :to="item.route"
              >
                <v-list-item-title>{{ item.title }}</v-list-item-title>
              </v-list-item>
              <v-list-item href="https://elearn.designwaycourses.com">
                <v-list-item-title>التعليم الالكتروني</v-list-item-title>
              </v-list-item>
            </v-list-item-group>
          </v-list>
        </v-menu>
      </template>

      <!-- normal navigation menu for sm devices -->

      <div v-else>
        <v-tabs
          optional
          background-color="transparent"
        >
          <v-tab
            href="https://elearn.designwaycourses.com"
            class="font-weight-bold"
            :ripple="false"
            min-width="96"
          >
            التعليم الالكتروني
          </v-tab>

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
