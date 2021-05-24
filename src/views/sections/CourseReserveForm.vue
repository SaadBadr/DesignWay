<template>
  <base-section id="pro-features">
    <v-img
      :src="require('@/assets/logo.png')"
      class="mx-auto mb-8"
      max-width="256"
    />
    <base-section-heading title="حجز الكورسات" />
    <v-card
      outlined
      max-width="1000"
      class="mx-auto pa-12"
    >
      <div v-if="nextPage === false">
        <form>
          <v-text-field
            v-model="firstName"
            flat
            reverse
            :error-messages="firstNameErrors"
            label="الاسم الأول"
            required
            @input="$v.firstName.$touch()"
            @blur="$v.firstName.$touch()"
          />

          <v-text-field
            v-model="lastName"
            flat
            reverse
            :error-messages="lastNameErrors"
            label="الاسم الأخير"
            required
            @input="$v.lastName.$touch()"
            @blur="$v.lastName.$touch()"
          />

          <v-text-field
            v-model="phone"
            reverse
            :error-messages="phoneErrors"
            label="رقم الموبيل"
            :counter="11"
            required
            @input="$v.phone.$touch()"
            @blur="$v.phone.$touch()"
          />

          <v-select
            v-model="selected_course"
            reverse
            :items="courses"
            :error-messages="selectCourseErrors"
            label="الكورسات"
            required
            @input="$v.selected_course.$touch()"
            @blur="$v.selected_course.$touch()"
          />

          <div v-if="selected_course !== null">
            <v-chip
              class="ma-2"
              color="orange"
              text-color="white"
            >
              سعر الحجز {{ price }} ج.م
              <v-icon right>
                mdi-star
              </v-icon>
            </v-chip>

            <v-card-subtitle>
              يتم خصم سعر الحجز من اجمالي قيمة الكورس
            </v-card-subtitle>
          </div>
          <v-container class="d-flex justify-center">
            <v-checkbox
              v-model="online"
              label="عايز الكورس اونلاين؟"
              class="d-flex justify-end"
            />
          </v-container>
          <v-select
            v-model="selected_city"
            reverse
            :items="Object.keys(centers)"
            :error-messages="selectCityErrors"
            label="المحافظة"
            :disabled="online"
            required
            @input="$v.selected_city.$touch()"
            @blur="$v.selected_city.$touch()"
          />

          <v-select
            v-model="selected_center"
            reverse
            :items="centers[selected_city]"
            :error-messages="selectCenterErrors"
            label="المنطقة"
            required
            :disabled="selected_city == null || online"
            @input="$v.selected_center.$touch()"
            @blur="$v.selected_center.$touch()"
          />

          <v-text-field
            v-model="code"
            reverse
            label="ٌReferal Code * (optional)"
          />

          <v-btn
            outlined
            color="success"
            class=" mr-4"
            @click="submit"
          >
            متابعة
          </v-btn>
          <v-btn
            outlined
            color="error"
            class="mr-4"
            @click="clear"
          >
            إعادة
          </v-btn>
        </form>
      </div>
      <div v-else>
        <h1 class="text-center mb-8">
          <u> تفاصيل الحجز </u>
        </h1>
        <h4 class="text-right my-5">
          {{ `${firstName} ${lastName}` }}
        </h4>
        <h4 class="text-right my-5">
          {{ phone }}
        </h4>
        <h4 class="text-right my-5">
          {{ code }}
        </h4>

        <h4 class="text-right my-5">
          {{ selected_course }} {{ online ? 'اونلاين' : selected_center }}
        </h4>

        <h4
          v-if="!online"
          class="text-right my-5"
        >
          {{ selected_city }}: {{ selected_center }}
        </h4>

        <h4 class="text-center my-5">
          <u> سعر الحجز: {{ price }} ج.م </u>
        </h4>

        <v-btn
          outlined
          color="success"
          class=" mr-4"
          :disabled="waitingForApiResponse"
          @click="payment"
        >
          استمرار للدفع
        </v-btn>
        <v-btn
          :disabled="waitingForApiResponse"
          outlined
          color="error"
          class="mr-4"
          @click="nextPage = false"
        >
          عودة
        </v-btn>

        <h6 class="text-right my-10">
          في حالة وجود مشكلة في الدفع تقدر تكلمنا او تبعتلنا على الواتساب هنا
          <a href="tel:01091752592">01091752592</a>
        </h6>
      </div>
    </v-card>
  </base-section>
</template>

<script>
  import { validationMixin } from 'vuelidate'
  import { required, maxLength, numeric } from 'vuelidate/lib/validators'
  import axios from 'axios'
  export default {
    name: 'SectionCourseReservation',
    components: {},
    mixins: [validationMixin],

    validations: {
      firstName: { required, maxLength: maxLength(15) },
      lastName: { required, maxLength: maxLength(15) },
      selected_course: { required },
      selected_city: {},
      selected_center: {},
      phone: { required, numeric, maxLength: maxLength(11) },
    },

    data: () => ({
      firstName: '',
      lastName: '',
      code: '',
      phone: '',
      waitingForApiResponse: false,
      nextPage: false,
      selected_course: null,
      selected_city: null,
      selected_center: null,
      courses: [
        'فنون تطبيقية',
        'فنون جميلة',
        'فنون عمارة',
        'تربية فنية',
        'اعلام',
      ],
      centers: {
        القاهرة: [
          'وسط البلد',
          'رمسيس',
          'مدينة نصر',
          'المعادي',
          'شبرا',
          'العبور',
          'حلمية الزيتون',
          'حلوان',
          'حدايق حلوان',
        ],
        الجيزة: ['الدقي', 'اكتوبر', 'الهرم', 'حدايق الأهرام'],
        الاسكندرية: ['سيدي بشر'],
        المنصورة: ['المنصورة'],
      },

      online: false,
    }),

    computed: {
      price () {
        return 50
      },
      selectCourseErrors () {
        const errors = []
        if (!this.$v.selected_course.$dirty) return errors
        !this.$v.selected_course.required &&
          errors.push('من فضلك قم باختيار مناسب')
        return errors
      },
      selectCityErrors () {
        const errors = []
        if (!this.$v.selected_city.$dirty) return errors
        !(this.$v.selected_city.required || !this.online) &&
          errors.push('من فضلك قم باختيار مناسب')
        return errors
      },
      selectCenterErrors () {
        const errors = []
        if (!this.$v.selected_center.$dirty) return errors
        !(this.$v.selected_center.required || !this.online) &&
          errors.push('من فضلك قم باختيار مناسب')
        return errors
      },

      firstNameErrors () {
        const errors = []
        if (!this.$v.firstName.$dirty) return errors
        !this.$v.firstName.maxLength &&
          errors.push('لا يجب ان يزيد الاسم عن 15 حرف')
        !this.$v.firstName.required && errors.push('من فضلك ادخل اسمك الاول')
        return errors
      },
      lastNameErrors () {
        const errors = []
        if (!this.$v.lastName.$dirty) return errors
        !this.$v.lastName.maxLength &&
          errors.push('لا يجب ان يزيد الاسم عن 15 حرف')
        !this.$v.lastName.required && errors.push('من فضلك ادخل اسمك الاخير')
        return errors
      },

      phoneErrors () {
        const errors = []
        if (!this.$v.phone.$dirty) return errors
        !this.$v.phone.maxLength && errors.push('لا يجب ان يزيد الرقم عن 11 حرف')
        !this.$v.phone.required && errors.push('من فضلك ادخل رقمك')
        !this.$v.phone.numeric && errors.push('تأكد من كتابة رقم صحيح')
        return errors
      },
    },
    methods: {
      submit () {
        this.$v.$touch()
        if (
          this.$v.$invalid ||
          (!this.online && (!this.selected_center || !this.selected_city))
        ) {
          return
        }
        this.nextPage = true
        this.waitingForApiResponse = false
      },
      clear () {
        this.$v.$reset()
        this.firstName = ''
        this.lastName = ''
        this.code = ''
        this.waitingForApiResponse = false
        this.selected_course = null
        this.selected_city = null
        this.selected_center = null
        this.online = false
      },
      async payment () {
        this.waitingForApiResponse = true
        var data = new FormData()
        data.append('vendorKey', process.env.VUE_APP_FAWATERK_API_KEY)
        data.append('cartItems[0][name]', `${this.selected_course} - حجز قدرات`)
        data.append('cartItems[0][price]', this.price)
        data.append('cartItems[0][quantity]', '1')
        data.append('cartItems[1][name]', 'مصاريف ادارية')
        data.append('cartItems[1][price]', 5)
        data.append('cartItems[1][quantity]', '1')
        data.append('cartTotal', this.price + 5)
        data.append('shipping', '0')
        data.append('customer[first_name]', this.firstName)
        data.append('customer[last_name]', this.lastName)
        data.append('customer[email]', this.code || '')
        data.append('customer[phone]', this.phone)
        data.append(
          'customer[address]',
          this.online
            ? 'اونلاين'
            : `${this.selected_center} - ${this.selected_city}`,
        )
        data.append('redirectUrl', 'http://www.designwaycourses.com/')
        data.append('currency', 'EGP')

        var config = {
          method: 'post',
          url: 'https://app.fawaterk.com/api/invoice',
          headers: {
            'Content-Type': 'multipart/form-data',
            'Accept-Language': '*',
          },
          data: data,
        }

        try {
          const response = await axios(config)
          if (response.data.error) throw new Error('من فضلك حاول مرة اخرى')
          window.location.href = response.data.url
        } catch (error) {
          alert(error.message)
          this.waitingForApiResponse = false
        }
      },
    },
  }
</script>
