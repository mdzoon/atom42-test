<template>
  <div>
    <b-jumbotron bg-variant="light" border-variant="dark" class="container mt-5" header="AtomGYM LTD" lead="We are dedicated to your mind and body fitness">

      <hr class="my-4">
      <div class="login-wrapper">
        <p>Sign up to our membership</p>
        <b-button variant="danger" class="mt-2 mb-5" @click="openedForm = !openedForm">Sign Up</b-button>
        <div v-if="!openedForm"> Already have account? Log in <a href="#">here</a>.</div>
      </div>
      <div class="mt-4 locations-container" v-if="openedForm">
        <membershipForm />
      </div>
    </b-jumbotron>
  </div>
</template>

<script>
import membershipForm from '@/components/membershipForm';

export default {
  name: 'homePage',
  components: {
    membershipForm // The original id was 1 - apparent typo
  },
  data: function() {
    return {
      openedForm: false
    }
  },
}
</script>
