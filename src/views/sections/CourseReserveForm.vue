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

          <v-text-field
            v-model="email"
            reverse
            :error-messages="emailErrors"
            label="الايميل"
            required
            @input="$v.email.$touch()"
            @blur="$v.email.$touch()"
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
              سعر الحجز 50 ج.م
              <v-icon right>
                mdi-star
              </v-icon>
            </v-chip>

            <v-card-subtitle>
              يتم خصم سعر الحجز من اجمالي قيمة الكورس
            </v-card-subtitle>
          </div>

          <v-select
            v-model="selected_city"
            reverse
            :items="Object.keys(centers)"
            :error-messages="selectCityErrors"
            label="المحافظة"
            required
            @input="$v.selected_city.$touch()"
            @blur="$v.selected_city.$touch()"
          />

          <v-select
            v-model="selected_center"
            reverse
            :items="centers[selected_city]"
            :error-messages="selectCenterErrors"
            label="السنتر"
            required
            :disabled="selected_city == null"
            @input="$v.selected_center.$touch()"
            @blur="$v.selected_center.$touch()"
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
          {{ name }}
        </h4>
        <h4 class="text-right my-5">
          {{ phone }}
        </h4>
        <h4 class="text-right my-5">
          {{ email }}
        </h4>

        <h4 class="text-right my-5">
          {{ selected_course }} - {{ selected_center }}
        </h4>

        <h4 class="text-right my-5">
          {{ selected_city }}: {{ selected_center }}
        </h4>

        <h4 class="text-center my-5">
          <u>
            سعر الحجز: 50 ج.م
          </u>
        </h4>
        <v-btn
          outlined
          color="success"
          class=" mr-4"
          @click="payment"
        >
          استمرار للدفع
        </v-btn>
        <v-btn
          outlined
          color="error"
          class="mr-4"
          @click="nextPage = false"
        >
          عودة
        </v-btn>
      </div>
    </v-card>
  </base-section>
</template>

<script>
  import { validationMixin } from 'vuelidate'
  import { required, maxLength, email, numeric } from 'vuelidate/lib/validators'
  import axios from 'axios'

  export default {
    name: 'SectionCourseReservation',
    components: {},
    mixins: [validationMixin],

    validations: {
      firstName: { required, maxLength: maxLength(15) },
      lastName: { required, maxLength: maxLength(15) },
      email: { required, email },
      selected_course: { required },
      selected_city: { required },
      selected_center: { required },
      phone: { required, numeric, maxLength: maxLength(11) },
    },

    data: () => ({
      features: [
        ['Components', 5, '40+'],
        ['Example Pages', 3, '10+'],
        ['Vue CLI Support', true, true],
        ['Bugfixes and Issues', false, true],
        ['6 Months Free Updates', false, true],
        ['Supports Vuetify', false, true],
        ['Price', 'Free', '$59'],
      ],
      firstName: '',
      lastName: '',
      email: '',
      phone: '',
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
          'الدقي - سنتر البسملة',
          'المعادي - سنتر الرحمة',
          'شبرا - سنتر هاني بيير',
          'العبور - سنتر',
          'حدايق الزيتون - سنتر',
          'حلوان - سنتر',
          'حدايق حلوان - سنتر',
        ],
        الجيزة: [
          'اكتوبر - سنتر الأمل',
          'الهرم - سنتر بسملة',
          'حدايق الأهرام -  سنتر بسملة',
        ],
        الاسكندرية: ['سيدي بشر - سنتر كوليدج'],
        المنصورة: ['ابن خالدون - سنتر سمارت'],
      },

      checkbox: false,
    }),

    computed: {
      checkboxErrors () {
        const errors = []
        if (!this.$v.checkbox.$dirty) return errors
        !this.$v.checkbox.checked && errors.push('You must agree to continue!')
        return errors
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
        !this.$v.selected_city.required && errors.push('من فضلك قم باختيار مناسب')
        return errors
      },
      selectCenterErrors () {
        const errors = []
        if (!this.$v.selected_center.$dirty) return errors
        !this.$v.selected_center.required &&
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
      emailErrors () {
        const errors = []
        if (!this.$v.email.$dirty) return errors
        !this.$v.email.email && errors.push('تأكد من كتابة بريد الالكتروني صحيح')
        !this.$v.email.required && errors.push('من فضلك ادخل البريد الالكتروني')
        return errors
      },
    },

    methods: {
      submit () {
        console.log(console.log(process.env.VUE_APP_FAWATERK_API_KEY))
        this.$v.$touch()

        console.log(this.$v.$invalid)
        if (this.$v.$invalid) return
        this.nextPage = true
      },
      clear () {
        this.$v.$reset()
        this.firstName = ''
        this.lastName = ''
        this.email = ''
        this.selected_course = null
        this.selected_city = null
        this.selected_center = null
        this.checkbox = false
      },
      payment () {
        var data = new FormData()
        data.append(
          'vendorKey',
          '39c7b8bee112c83c763ea7172db149ea69839b49be545ccacb',
        )
        data.append('cartItems[0][name]', `${this.selected_course} - حجز قدرات`)
        data.append('cartItems[0][price]', '50')
        data.append('cartItems[0][quantity]', '1')
        data.append('cartTotal', '50')
        data.append('shipping', '0')
        data.append('customer[first_name]', this.firstName)
        data.append('customer[last_name]', this.lastName)
        data.append('customer[email]', this.email)
        data.append('customer[phone]', this.phone)
        data.append(
          'customer[address]',
          `${this.selected_center} - ${this.selected_city}`,
        )
        data.append('redirectUrl', 'https://www.fawaterk.com')
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

        axios(config)
          .then(function (response) {
            console.log(JSON.stringify(response.data))
            window.location.href = response.data.url
          })
          .catch(function (error) {
            console.log(error)
          })
      },
    },
  }
</script>
