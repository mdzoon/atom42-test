<template>
  <div>

    <b-form @submit="onSubmit" @reset="onReset">

      <div v-if="tab === 1">
        <h3>Select your local gym:</h3>
        <gyms class="mt-4" v-for="gym in gymData" :key="gym.id" :gym="gym" @update-gym="updateGym"/>
      </div>

      <div v-if="tab === 2">

        <b-form-group
          id="first-name-group"
          label="Your First Name *"
          label-for="first-name"
          :class="{invalid: formSubmitted && $v.form.firstName.$invalid}"
        >
          <b-form-input
            id="first-name"
            v-model="$v.form.firstName.$model"
            placeholder="Enter First Name"
          ></b-form-input>
          <p class="alert alert-danger mt-1" role="alert" v-if="formSubmitted && !$v.form.firstName.required">
            This field shoud not be empty!
          </p>
          <p class="alert alert-danger mt-1" role="alert" v-if="formSubmitted && !$v.form.firstName.minLength">
            Your name must contain at least {{$v.form.firstName.$params.minLength.min}} letters.
          </p>
        </b-form-group>

        <b-form-group
          id="last-name-group"
          label="Your Last Name *"
          label-for="last-name"
          :class="{invalid: formSubmitted && $v.form.lastName.$invalid}"
        >
          <b-form-input
            id="last-name"
            v-model="$v.form.lastName.$model"  
            placeholder="Enter Last Name"
          ></b-form-input>
          <p class="alert alert-danger mt-1" role="alert" v-if="formSubmitted && !$v.form.lastName.required">
            This field shoud not be empty!
          </p>
          <p class="alert alert-danger mt-1" role="alert" v-if="formSubmitted && !$v.form.lastName.minLength">
            Your name must contain at least {{$v.form.lastName.$params.minLength.min}} letters.
          </p>
        </b-form-group>

        <b-form-group
          id="email-group"
          label="Email address *"
          label-for="email"
          :class="{invalid: formSubmitted && $v.form.email.$invalid}"
        >
          <b-form-input
            id="email"
            v-model="$v.form.email.$model"
            type="email"
            placeholder="Enter Your Email"
          ></b-form-input>
          <p class="alert alert-danger mt-1" role="alert" v-if="formSubmitted && !$v.form.email.required">
            This field shoud not be empty!
          </p>
          <p class="alert alert-danger mt-1" role="alert" v-if="formSubmitted && !$v.form.email.email">
            Please provide valid email!
          </p>
        </b-form-group>

        <b-form-group
          id="phone-group"
          label="Phone number (UK Only)"
          label-for="phone"
          :class="{invalid: formSubmitted && $v.form.phone.$invalid}"
        >
          <b-form-input
            id="phone"
            v-model="$v.form.phone.$model"
            type="tel"
            placeholder="Enter Your Phone"
          ></b-form-input>
          <p class="alert alert-danger mt-1" role="alert" v-if="formSubmitted && !$v.form.phone.numeric">
            Please enter only numbers!
          </p>
          <p class="alert alert-danger mt-1" role="alert" v-if="formSubmitted && !$v.form.phone.minLength">
            The number must contain at least {{$v.form.phone.$params.minLength.min}} digits.
          </p>
        </b-form-group>

        <b-form-group
          id="postcode-group"
          label="Address (UK Postcode)"
          label-for="postcode"
          :class="{invalid: formSubmitted && $v.form.postcode.$invalid}"
        >
          <b-form-input
            id="postcode"
            v-model="form.postcode"
            type="text"
            placeholder="Enter Your Postcode"
          ></b-form-input>
          <p class="alert alert-danger mt-1" role="alert" v-if="formSubmitted && !$v.form.postcode.minLength">
            The postcode must contain at least {{$v.form.postcode.$params.minLength.min}} letters and numbers.
          </p>
        </b-form-group>

        <b-form-group
          id="age-group"
          label="Age (must be older than 18) *"
          label-for="age"
          :class="{invalid: formSubmitted && $v.form.age.$invalid}"
        >
          <b-form-input
            id="age"
            v-model="form.age"
            type="number"
            placeholder="Enter Your Age"
          ></b-form-input>
          <p class="alert alert-danger mt-1" role="alert" v-if="formSubmitted && !$v.form.age.minValue">
            You must be over {{$v.form.age.$params.minValue.min}} years old.
          </p>
        </b-form-group>

        <b-form-group id="consent-group" v-slot="{ ariaDescribedby }">
          <b-form-checkbox
            id="consent"
            name="consent"
            :aria-describedby="ariaDescribedby"
            v-model="form.consent"
            value="yes"
            unchecked-value="no"
            :class="{invalid: formSubmitted && $v.form.consent.$invalid}"
          >
            I accept the T&C's *
          </b-form-checkbox>
          <p class="alert alert-danger mt-1" role="alert" v-if="formSubmitted && !$v.form.consent.$model">
            Please confirm you agree to our T&C!
          </p>
        </b-form-group>

        <b-form-group id="newsletter-group" v-slot="{ ariaDescribedby }">
          <b-form-checkbox
            id="newsletter"
            name="newsletter"
            :aria-describedby="ariaDescribedby"
            v-model="form.newsletter"
            value="yes"
            unchecked-value="no"
          >
            Subscribtion to the newsletter
          </b-form-checkbox>
        </b-form-group>

      </div>

      <div v-if="tab === 3">

        <div class="mt-3">
          <p class="alert alert-danger" role="alert" v-if="submitStatus === 'ERROR'">Please fill in the form correctly.</p>
          <p class="alert alert-info" role="alert" v-if="submitStatus === 'PENDING'">Sending...</p>
        </div>

        <b-card class="mt-3" header="Your details:">
          <ul>
            <summaryPage class="mt-1" v-for="(entry, name, index) in form" :key="index" :entry="entry" :name="name" />
          </ul>
        </b-card>

        <div class="mt-4 buttons-container">
          <b-button type="submit" variant="primary">Confirm</b-button>
          <b-button type="reset" variant="danger">Reset</b-button>
        </div>
      </div>

    </b-form>

    <div class="mt-4 buttons-container" v-if="tab !== 4">
      <b-button variant="outline-dark" @click.prevent="previous()" v-show="!isFirst()">Previous</b-button>
      <b-button variant="outline-dark" @click.prevent="next()" v-show="!isLast()">Next</b-button>
    </div>

    <h1 class="p-5 alert alert-success" role="alert" v-if="submitStatus === 'OK' && tab === 4">Thank you for your submission!</h1>

  </div>
</template>

<script>
import { required, minLength, email, numeric, minValue } from 'vuelidate/lib/validators'
import gymData from "@/assets/data/gym.json";
import gyms from "@/components/gyms";
import summaryPage from "@/components/summaryPage";

export default {
  name: 'membershipForm',
  data () {
    return {
      gymData,
      openedForm: false,
      tab: 1,
      form: {
        firstName: '',
        lastName: '',
        email: '',
        phone: '',
        postcode: '',
        age: '',
        location: '',
        plan: '',
        consent: '',
        newsletter: ''
      },
      show: false,
      submitStatus: '',
      formSubmitted: false,
      // action: '' TODO call to API
    }
  },
  components: {
    gyms,
    summaryPage
  },
	methods: {
    onSubmit(event) {
      event.preventDefault()
      this.$v.form.$touch()
      if (this.$v.form.$invalid || !this.$v.form.consent.$model) {
        this.submitStatus = 'ERROR'
        this.formSubmitted = true
      } else {
        this.submitStatus = 'PENDING'

        // TODO - basic XMLHttpRequest
        // const form = this
        // const xhr = new XMLHttpRequest();
        // xhr.open('POST', this.form.action, false);
        // // xhr.withCredentials = true;
        // xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
        // xhr.onreadystatechange = function() {
        //   form.submitStatus = 'PENDING'
        //   if (xhr.readyState === 4 && xhr.status === 200) {
        //     form.submitStatus = 'OK'
        //     alert(JSON.stringify(form.form))
        //     this.tab = 4;
        //     this.onReset();
        //   }
        // }
        // xhr.send();

        setTimeout(() => {
          this.submitStatus = 'OK'
          this.tab = 4;
        }, 500)

      }
    },
    onReset(event) {
      event.preventDefault()
      // Reset our form values
      this.form.firstName = ''
      this.form.lastName = ''
      this.form.email = ''
      this.form.phone = ''
      this.form.postcode = ''
      this.form.age = ''
      this.form.location = ''
      this.form.plan = ''
      this.form.consent = '',
      this.form.newsletter = '' 
      // Trick to reset/clear native browser form validation state
      this.show = false
      this.$nextTick(() => {
        this.show = true
      })
    },
		next() {
			this.tab++
		},
		previous() {
			this.tab--
		},
    isFirst() {
      return this.tab === 1
    },
    isLast() {
      return this.tab === 3
    },
    updateGym(gymName, gymPlan) {
      this.form.location = gymName;
      this.form.plan = gymPlan;
    }
  },
  validations: {
    form: {
      firstName: {
        required,
        minLength: minLength(3)
      },
      lastName: {
        required,
        minLength: minLength(3)
      },
      email: {
        required,
        email
      },
      phone: {
        numeric,
        minLength: minLength(10)
      },
      postcode: {
        minLength: minLength(6)
      },
      age: {
        minValue: minValue(18)
      },
      consent: {
        required
      }
    }
  }
}
</script>


<style>
.buttons-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

button,
.buttons-container button {
  min-width: 10rem;
}
</style>