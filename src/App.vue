<template>
  <div>
    <h3>Selected: {{selectedPerson}}</h3>
    <x-select
      :options="[{ id:1, name: 'John' }, { id:2, name: 'Phil' }, { id:3, name: 'paul' }]"
      v-model="selectedPerson"
      label-from="name"
      value-from="id"
    >
  </div>
</template>

<script>
import xSelect from './components/Select'

export default {
  components: {
    xSelect
  },
  data() {
    return {
      selectedPerson: 2,
    }
  }
}
</script>
