<template>
  <section>
    <div class="container">
      <items-list />
    </div>
  </section>
</template>

<script>
import ItemsList from '../components/ItemsList.vue'

export default {
  components: { ItemsList },
  setup() {
    return {}
  },
}
</script>