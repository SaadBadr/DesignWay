<template>
  <base-section id="current-courses">
    <base-section-heading title="الكورسات الحالية" />

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
          />
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
          text: 'طريقك للتصميم والهندسة لمختلف المجالات في الحياة',
        },
        {
          title: 'فنون جميلة',
          text: 'اقصر طريق للفن و الجمال والذوق في تكوين شخصيه الفنان',
        },
        {
          color: 'primary',
          dark: true,
          title: 'فنون عمارة',
          text:
            'هنا المزيج بين الفن والهندسة لاخراج مهندس معماري مميز ملم بجميع النواحي الفنية',
        },
        {
          title: 'تربية فنية',
          text: 'لأن الفن رساله فدورك توصله وتطور فيه',
        },
        {
          title: 'اعلام',
          text: 'طريق الشهرة وتصحيح فكر الشعوب والمعرفة بكل بلاد العالم',
        },
      ],
    }),
  }
</script>
