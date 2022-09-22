<template>
  <h3>{{ event.organizer.name }}</h3>
  <p>{{ event.time }} on {{ event.date }} @ {{ event.location }}</p>
  <p>{{ event.description }}</p>
</template>

<script>
export default {
  props: ['id', 'event']
}
</script>
