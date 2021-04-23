<script>
// Extensions
  import View from '@/views/View'

  // Mixins
  import LoadSections from '@/mixins/load-sections'

  export default {
    name: 'CourseReservation',

    metaInfo: { title: 'Course Reservation' },

    extends: View,

    mixins: [LoadSections(['course-reserve-form'])],

    props: {
      id: {
        type: String,
        default: 'course-reservation',
      },
    },
  }
</script>
