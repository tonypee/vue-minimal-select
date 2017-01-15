<template>
  <div class="row">
    <div class="six columns">
      <h2>Single Select</h2>
      <h4>Selected: {{selectedPerson}}</h4>
      <x-select
        :options="[{ id:1, name: 'John' }, { id:2, name: 'Phil' }, { id:3, name: 'paul' }]"
        v-model="selectedPerson"
        label-from="name"
        value-from="id"
      >
      <hr />
    </div>

    <div class="six columns">
      <h2>Multi Select</h2>
      <h4>Selected: {{selectedPeople}}</h4>
      <x-select
        :options="[{ id:1, name: 'John' }, { id:2, name: 'Phil' }, { id:3, name: 'paul' }]"
        v-model="selectedPeople"
        :multiple="true"
        label-from="name"
        value-from="id"
      >
    </div>
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
      selectedPeople: [],
    }
  }
}
</script>
