<template>
  <div>
    <b-button
      :class="visible ? null : 'collapsed'"
      :aria-expanded="visible ? 'true' : 'false'"
      :aria-controls="`collapse-${gym.id}`"
      @click="visible = !visible"
      variant="primary">{{ gym.name }}</b-button>
    <b-collapse :id="`collapse-${gym.id}`" v-model="visible" class="mt-2">
      <b-card>
        <h3>Select your plan:</h3>
        <b-form-group label="Individual radios">
          <plans v-for="(plan, index) in gym.plans" :key="index" :plan="plan" />
        </b-form-group>
      </b-card>
    </b-collapse>
  </div>
</template>

<script>
import plans from "@/components/plans";

export default {
  name: "gym",
  components: {
    plans
  },
  props: { 
    gym: Object
  },
  data() {
    return {
      visible: false
    }
  }
}
</script>
