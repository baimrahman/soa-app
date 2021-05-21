<template>
  <div>
    <b-button to="/">Home</b-button>
    <b-table :items="items" :fields="fields"></b-table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      items: [],
      fields: [],
    }
  },
  created() {
    this.$axios
      .get(
        'https://soa-server.herokuapp.com/form/data/' + this.$route.params.id
      )
      .then((res) => {
        for (let i = 0; i < res.data.length; i++) {
          let a = {}
          for (let j = 0; j < res.data[i].items.length; j++) {
            a[res.data[i].items[j].id] = res.data[i].items[j].value
          }

          this.items.push(a)
        }
        for (let i = 0; i < res.data[0].items.length; i++) {
          const b = {
            key: res.data[0].items[i].id,
            label: res.data[0].items[i].label,
          }
          this.fields.push(b)
        }
        console.log(this.fields)
      })
  },
}
</script>

<style>
</style>