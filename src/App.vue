<template>
  <div class="row">
    <div class="four columns">
      <h2>Single Select</h2>
      <h4>Selected: {{selectedPerson}}</h4>
      <x-select
        :options="data"
        v-model="selectedPerson"
        label-from="name"
        value-from="id"
        :show-search="true"
      >
      <hr />
    </div>

    <div class="four columns">
      <h2>Multi Select</h2>
      <h4>Selected: {{selectedPeople}}</h4>
      <x-select
        :options="data"
        v-model="selectedPeople"
        :multiple="true"
        label-from="name"
        value-from="id"
      >
    </div>

    <div class="four columns">
    <h2>Simple Data</h2>
      <h4>Selected: {{selectedNumbers}}</h4>
      <x-select
        :options="['one', 'two', 'three']"
        v-model="selectedNumbers"
        :multiple="true"
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
      data: [{ id: 1, name: 'John' }, { id: 2, name: 'Phil' }, { id: 3, name: 'paul' }],
      selectedPerson: 2,
      selectedPeople: [],
      selectedNumbers: [],
    }
  }
}
</script>
