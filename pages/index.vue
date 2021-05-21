<template>
  <div class="d-flex">
    <b-col class="box p-3">
      <b-button class="mb-3" to="/create">CREATE NEW</b-button>
      <div
        v-for="(i, index) in items"
        :key="index"
        class="d-flex justify-content-between align-content-center item-box p-3 mb-3"
      >
        <div>{{ i.title }}</div>
        <div>
          <b-button class="mr-2" :to="`/submit/${i._id}`" size="sm"
            >Make Data</b-button
          >
          <b-button class="mr-2" :to="`/table/${i._id}`" size="sm"
            >See Data</b-button
          >
          <b-button
            class="mr-2"
            variant="danger"
            @click="deleteData(i._id)"
            size="sm"
            >Delete</b-button
          >
        </div>
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
    this.getData()
  },
  methods: {
    deleteData(id) {
      this.$axios
        .delete(`https://soa-server.herokuapp.com/form/${id}`)
        .then((res) => {
          this.getData()
        })
    },
    getData() {
      this.$axios.get('https://soa-server.herokuapp.com/form').then((res) => {
        this.items = res.data
      })
    },
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