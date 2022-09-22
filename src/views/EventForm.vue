<template>
  <div>
    <h1>Create an event</h1>
    <form @submit.prevent="saveEvent">
      <BaseInput v-model="event.category" type="text" label="Category" />
      <h3>Name & describe your event</h3>

      <BaseInput v-model="event.title" type="text" label="Title" />

      <h3>Who is your organizer?</h3>
      <label>Select an Organizer</label>
      <select v-model="event.organizer.id">
        <option
          v-for="option in GStore.organizers"
          :value="option.id"
          :key="option.id"
          :selected="option.id === event.organizer.id"
        >
          {{ option.name }}
        </option>
      </select>
      <label>Description</label>
      <input
        v-model="event.description"
        type="text"
        placeholder="Description"
        class="field"
      />

      <h3>Where is your event?</h3>

      <label>Location</label>
      <input
        v-model="event.location"
        type="text"
        placeholder="Location"
        class="field"
      />
      <button type="submit">Submit</button>
    </form>

    <pre>{{ event }}</pre>
  </div>
</template>

<script>
import EventService from '@/services/EventService.js'
export default {
  inject: ['GStore'],
  data() {
    return {
      event: {
        category: '',
        title: '',
        description: '',
        location: '',
        organizer: { id: '', name: '' }
      }
    }
  },
  methods: {
    saveEvent() {
      EventService.saveEvent(this.event)
        .then((response) => {
          console.log(response)
          this.$router.push({
            name: 'EventLayoutView',
            params: { id: response.data.id }
          })
          this.GStore.flashMessage =
            'You are succcessfully add a new event for ' + response.data.title
          setTimeout(() => {
            this.GStore.flashMessage = ''
          }, 3000)
        })
        .catch(() => {
          this.$router.push('NetworkError')
        })
    }
  }
}
</script>
