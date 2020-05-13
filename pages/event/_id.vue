<template>
  <div>
    <h1>{{ event.title }}</h1>
  </div>
</template>

<script>
export default {
  async asyncData({ $axios, error, params }) {
    const eventId = params.id;
    try {
      const { data } = await $axios.get(
        `http://localhost:3100/events/${eventId}`
      );
      return {
        event: data
      };
    } catch (e) {
      error({
        statusCode: 503,
        message: `Unable to fetch event #${eventId}. Please try again!`
      });
    }
  },
  computed: {
    id() {
      return this.$route.params.id;
    }
  },
  head() {
    return {
      title: `${this.event.title}`,
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: `What you need to know about ${this.event.title}`
        }
      ]
    };
  }
};
</script>
