<template>
  <v-theme-provider dark>
    <base-section
      id="social"
      class="accent text-center"
      space="56"
    >
      <base-icon class="mb-8">
        mdi-facebook
      </base-icon>

      <base-section-heading
        color="transparent"
        title="اتواصل معانا"
      />

      <div class="py-4" />

      <v-btn
        class="font-weight-bold"
        color="white"
        href="https://facebook.com/design_way"
        light
        min-width="168"
        target="_blank"
        x-large
      >
        تابعنا

        <v-icon right>
          mdi-open-in-new
        </v-icon>
      </v-btn>
    </base-section>
  </v-theme-provider>
</template>

<script>
  export default {
    name: 'SectionNews',

    provide: {
      heading: { align: 'center' },
    },
  }
</script>
