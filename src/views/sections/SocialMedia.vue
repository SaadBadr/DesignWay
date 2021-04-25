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
        href="https://www.facebook.com/groups/251019392512811/?ref=share&exp=e8c2"
        light
        min-width="168"
        target="_blank"
        x-large
      >
        جروبنا على الفيسبوك

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
