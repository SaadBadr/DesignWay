<template>
  <base-section id="current-courses">
    <base-section-heading title="الكورسات الحالية">
      كورسات جامدة جدا
    </base-section-heading>

    <v-container>
      <v-row>
        <v-col
          v-for="(course, i) in courses"
          :key="i"
          cols="12"
          md="6"
        >
          <base-avatar-card
            v-bind="course"
            align="center"
          >
            كورس ممتاز جدا طبعاكورس ممتاز جدا طبعاكورس ممتاز جدا طبعاكورس ممتاز
            جدا طبعا
          </base-avatar-card>
        </v-col>
      </v-row>
    </v-container>
  </base-section>
</template>

<script>
  export default {
    name: 'CurrentCourses',

    data: () => ({
      courses: [
        {
          title: 'فنون تطبيقية',
        },
        {
          title: 'فنون جميلة',
        },
        {
          color: 'primary',
          dark: true,
          title: 'فنون عمارة',
        },
        {
          title: 'تربية فنية',
        },
        {
          title: 'اعلام',
        },
      ],
    }),
  }
</script>
