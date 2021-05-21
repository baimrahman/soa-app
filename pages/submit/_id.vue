<template>
  <div>
    <b-button to="/">Home</b-button>
    <div class="d-flex">
      <b-col class="box p-3">
        <div class="item-box p-3 mb-3" v-for="(i, index) in items" :key="index">
          <div>
            <label :for="i.id">{{ i.label }}</label>
          </div>
          <input
            class="w-100 mb-3"
            :type="i.type"
            :name="i.id"
            v-model="i.value"
          />
        </div>
        <b-button
          v-if="items.length > 0"
          @click="sendData()"
          class="w-100"
          variant="success"
          >SAVE DATA</b-button
        >
      </b-col>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      items: [],
    }
  },
  beforeCreate() {
    this.$axios
      .get('https://soa-server.herokuapp.com/form/' + this.$route.params.id)
      .then((res) => {
        console.log(res.data)
        this.items = res.data.items
      })
  },
  methods: {
    sendData() {
      const data = { dbId: this.$route.params.id, items: this.items }
      this.$axios
        .post('https://soa-server.herokuapp.com/form/submit', data)
        .then((res) => {
          this.$router.push('/')
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