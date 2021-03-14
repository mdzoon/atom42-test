<template>
  <div>
    <b-form-radio
      v-model="selected"
      :aria-describedby="ariaDescribedby"
      name="some-radios"
      :value="`${plan.name}:${plan.price}`"
      @input="$emit('input', $event.target.value)"
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
  }
}
</script>