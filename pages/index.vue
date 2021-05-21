<template>
  <div class="d-flex">
    <b-col class="box p-3">
      <b-button class="mb-3" to="/create">CREATE NEW</b-button>
      <div
        v-for="(i, index) in items"
        :key="index"
        class="d-flex item-box p-3 mb-3"
      >
        <div>{{ i._id }}</div>
        <b-button class="mr-2" :to="`/submit/${i._id}`" size="sm"
          >Make Data</b-button
        >
        <b-button :to="`/table/${i._id}`" size="sm">See Data</b-button>
      </div>
    </b-col>
  </div>
</template>

<script>
export default {
  data() {
    return {
      items: [],
    }
  },
  created() {
    this.$axios.get('https://soa-server.herokuapp.com/form').then((res) => {
      this.items = res.data
    })
  },
}
</script>

<style>
.box {
  background-color: #faf1e6;

  /* border: 1px solid black; */
}
.item-box {
  background: #e4efe7;
}
</style>