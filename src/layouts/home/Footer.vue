<template>
  <v-footer
    id="home-footer"
    color="black"
    dark
    min-height="72"
  >
    <v-container>
      <v-row>
        <v-col
          cols="12"
          md="6"
        >
          <div
            class="d-flex flex-wrap justify-md-start justify-center justify-md-none"
          >
            <template v-for="(url, title, i) in social">
              <a
                :key="title"
                class="white--text px-1 py-md-0"
                :href="url"
                target="_blank"
                v-text="title"
              />

              <v-responsive
                v-if="i < social.length - 1"
                :key="`divider-${title}`"
                class="mx-4 shrink hidden-sm-and-down"
                max-height="24"
              >
                <v-divider vertical />
              </v-responsive>
            </template>
          </div>
        </v-col>

        <v-col
          class="text-center text-md-right"
          cols="12"
          md="6"
        >
          Copyright &copy; {{ new Date().getFullYear() }}
          <a
            title="SaadBadr23@gmail.com"
            href="https://www.linkedin.com/in/saadbadr"
            target="_blank"
          >SaadBadr</a>
        </v-col>
      </v-row>
    </v-container>
  </v-footer>
</template>

<script>
  export default {
    name: 'HomeFooter',

    data: () => ({
      social: {
        Facebook: 'https://www.facebook.com/designawycourses/',
        Instagram: 'https://instagram.com/design.way_?igshid=16ba7q5s5hebm',
        Telegram: 'https://t.me/Design_way',
        Youtube: 'https://www.youtube.com/channel/UC-TKcZ4KYMT24tINpVqwYfg',
      },
    }),
  }
</script>

<style lang="sass">
#home-footer a
  text-decoration: none
</style>
