<template>
  <div>
    <h3>Select your local gym:</h3>

    <b-form @submit="onSubmit" @reset="onReset">

      <div v-if="tab === 1">
        <gyms class="mt-4" v-for="gym in gymData" :key="gym.id" :gym="gym" @update-gym="updateGym"/>
      </div>

      <div v-if="tab === 2">

        <b-form-group id="first-name-group" label="Your First Name:" label-for="first-name">
          <b-form-input
            id="first-name"
            v-model="form.firstName"
            placeholder="Enter First Name"
            required
          ></b-form-input>
        </b-form-group>

        <b-form-group id="last-name-group" label="Your Last Name:" label-for="last-name">
          <b-form-input
            id="last-name"
            v-model="form.lastName"
            placeholder="Enter Last Name"
            required
          ></b-form-input>
        </b-form-group>

        <b-form-group id="email-group" label="Email address:" label-for="email">
          <b-form-input
            id="email"
            v-model="form.email"
            type="email"
            placeholder="Enter Your Email"
            required
          ></b-form-input>
        </b-form-group>

        <b-form-group id="phone-group" label="Phone number (UK Only):" label-for="phone">
          <b-form-input
            id="phone"
            v-model="form.phone"
            type="tel"
            placeholder="Enter Your Phone"
            required
          ></b-form-input>
        </b-form-group>

        <b-form-group id="postcode-group" label="Address (UK Postcode):" label-for="postcode">
          <b-form-input
            id="postcode"
            v-model="form.postcode"
            type="text"
            placeholder="Enter Your Postcode"
            required
          ></b-form-input>
        </b-form-group>

        <b-form-group id="age-group" label="Age (must be older than 18):" label-for="age">
          <b-form-input
            id="age"
            v-model="form.age"
            type="number"
            placeholder="Enter Your Age"
            required
          ></b-form-input>
        </b-form-group>

        <b-form-group id="checkbox-group" v-slot="{ ariaDescribedby }">
          <b-form-checkbox-group
            v-model="form.checked"
            id="checkboxes"
            :aria-describedby="ariaDescribedby"
          >
            <b-form-checkbox value="consent">T&Cs checkbox</b-form-checkbox>
            <b-form-checkbox value="newsletter">Newsletter signup checkbox</b-form-checkbox>
          </b-form-checkbox-group>
        </b-form-group>

      </div>

      <div v-if="tab === 3">
        <b-card class="mt-3" header="Your details">
          <pre class="m-0">{{ form }}</pre>
        </b-card>

        <b-button type="submit" variant="primary">Confirm</b-button>
        <b-button type="reset" variant="danger">Reset</b-button>
      </div>

    </b-form>

    <div class="mt-4 buttons-container">
      <b-button variant="outline-dark" @click.prevent="previous()" v-show="!isFirst()">Previous</b-button>
      <b-button variant="outline-dark" @click.prevent="next()" v-show="!isLast()">Next</b-button>
    </div>

  </div>
</template>

<script>
import gymData from "@/assets/data/gym.json";
import gyms from "@/components/gyms";

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
          checked: []
        },
      show: false
    }
  },
  components: {
    gyms // The original id was 1 - apparent typo
  },
	methods: {
    onSubmit(event) {
      event.preventDefault()
      // submit logic
      alert(JSON.stringify(this.form))
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
      this.location = ''
      this.planPlan = ''
      this.form.checked = []
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
      this.form.gymName = gymName;
      this.form.plan = gymPlan;
    }
  }
}
</script>
