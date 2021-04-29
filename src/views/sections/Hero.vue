<template>
  <section id="hero">
    <v-img
      :min-height="minHeight"
      :src="require('@/assets/Cvr - Copy.png')"
      class="white--text"
      gradient="to bottom, rgba(11, 28, 61, .9), rgba(0,0,0,.4)"
    >
      <v-container class="fill-height px-4 py-12">
        <v-responsive
          class="d-flex align-center mx-auto"
          height="100%"
          max-width="700"
          width="100%"
        >
          <base-subheading
            class="text-right"
            weight="regular"
            title="طريقك لاجتيار"
          />

          <base-heading
            class="text-right"
            size="text-h2"
            title="امتحان القدرات"
            weight="medium"
          />

          <base-body class="text-right">
            الحق احجز مكانك دلوقتي في كورسات اختبارات كليات الفنون
          </base-body>

          <div
            :class="
              $vuetify.breakpoint.smAndDown
                ? 'flex-column align-'
                : 'align-right'
            "
            class="d-flex flex-wrap"
          >
            <v-spacer />
            <base-btn
              :ripple="false"
              class="pa-1"
              height="auto"
              text
              href="https://www.facebook.com/218476249036271/posts/768885087328715/?sfnsn=scwspmo"
              target="_blank"
            >
              معلومات اكتر؟
            </base-btn>

            <span class="font-weight-bold ml-6 mr-4 my-4" />
            <base-btn to="course-reservation">
              حجز الكورسات
            </base-btn>
          </div>
        </v-responsive>
      </v-container>
    </v-img>
  </section>
</template>

<script>
  export default {
    name: 'SectionHero',

    provide: {
      theme: { isDark: true },
    },

    computed: {
      minHeight () {
        const height = this.$vuetify.breakpoint.mdAndUp ? '100vh' : '50vh'

        return `calc(${height} - ${this.$vuetify.application.top}px)`
      },
    },
  }
</script>
