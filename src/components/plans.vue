<template>
  <div>
    <b-form-radio
      v-model="selected"
      :name="`${plan.name}-radios`"
      :value="`${plan.name}:${plan.price}`"
    >
      {{ plan.name }} : £{{ plan.price }}
    </b-form-radio>
  </div>
</template>

<script>

export default {
  name: "plan",
  props: { 
    plan: Object
  },
  data () {
    return {
      selected: ''
    }
  },
  watch: {
    selected () {
      this.$emit('update-plan', this.plan.name);
    }
  } 

}
</script>