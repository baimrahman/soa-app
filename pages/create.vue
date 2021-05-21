<template>
  <div>
    <b-modal id="modalEdit" hide-footer title="Edit Data">
      <label>Input label name</label>
      <input type="text" v-model="editName" />
      <b-button
        size="sm"
        variant="primary"
        @click="
          editHandler()
          $bvModal.hide('modalEdit')
        "
        >SAVE</b-button
      >
    </b-modal>
    <b-button to="/">Home</b-button>
    <div class="d-flex">
      <b-col cols="4" class="item-box px-3">
        <div
          class="p-3 model mt-3 grab"
          @dragstart="
            startDrag(
              $event,
              JSON.stringify({ type: 'text', label: 'Question' })
            )
          "
          draggable
        >
          <div>Text</div>
          <div class="d-flex justify-content-center">
            <input disabled class="w-100 grab" type="text" />
          </div>
        </div>
        <!-- <div>
          <input
            class="grab"
            @dragstart="
              startDrag(
                $event,
                JSON.stringify({ type: 'checkbox', label: 'Question' })
              )
            "
            draggable
            type="checkbox"
          />
        </div> -->
        <div
          class="p-3 model mt-3 grab"
          @dragstart="
            startDrag(
              $event,
              JSON.stringify({ type: 'date', label: 'Question' })
            )
          "
          draggable
        >
          <div>Date</div>
          <div class="d-flex justify-content-center">
            <input disabled class="w-100 grab" type="date" />
          </div>
        </div>
      </b-col>
      <b-col cols="8" class="box p-3">
        <b-form-input
          v-model="title"
          class="mb-3"
          placeholder="Input Title"
        ></b-form-input>
        <div
          @dragover.prevent
          @dragenter.prevent
          @drop="onDrop($event)"
          @dragover="allowDrop($event)"
          class="dropbox"
        >
          <div
            class="item-box p-3 mb-3"
            v-for="(i, index) in items"
            :key="index"
          >
            <div>
              <label :for="i.id">{{ i.label }}</label>
            </div>
            <input
              class="w-100 mb-3"
              :type="i.type"
              :name="i.id"
              v-model="i.value"
            />
            <div class="d-flex justify-content-end">
              <b-button
                class="mr-2"
                @click="
                  edit = i
                  editIndex = index
                  editName = null
                "
                variant="info"
                v-b-modal.modalEdit
                size="sm"
                >Edit</b-button
              >
              <b-button @click="deleteHandler(i.id)" variant="danger" size="sm"
                >Delete</b-button
              >
            </div>
          </div>
        </div>
        <b-button @click="sendData()" class="w-100" variant="success"
          >SAVE FORM</b-button
        >
      </b-col>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      title: null,
      items: [],
      edit: {},
      editIndex: null,
      editName: null,
    }
  },
  methods: {
    sendData() {
      this.$axios
        .post('https://soa-server.herokuapp.com/form', {
          title: this.title,
          items: this.items,
        })
        .then((res) => {
          console.log('Good')
          this.$router.push('/')
        })
        .catch((err) => {
          console.log(err)
        })
    },
    deleteHandler(id) {
      const index = this.items.findIndex((e) => e.id === id)
      this.items.splice(index, 1)
    },
    editHandler() {
      this.edit.label = this.editName
      this.items.splice(this.editIndex, 1, this.edit)
    },
    allowDrop(ev) {
      ev.preventDefault()
    },
    onDrop(evt) {
      const a = JSON.parse(evt.dataTransfer.getData('itemID'))

      const index = this.items.length

      this.items.push({
        id: `${index}-${a.type}`,
        type: a.type,
        label: a.label,
        value: null,
      })
    },
    startDrag: (evt, item) => {
      // evt.dataTransfer.dropEffect = 'move'
      // evt.dataTransfer.effectAllowed = 'move'
      evt.dataTransfer.setData('itemID', item)
    },
  },
}
</script>

<style>
.model {
  background-color: #faf1e6;
}
.item-box {
  background: #e4efe7;
}
.grab {
  cursor: -webkit-grab;
  cursor: grab;
}
.box {
  background-color: #faf1e6;

  /* border: 1px solid black; */
}
.dropbox {
  min-height: 500px;
  /* background: black; */
}
</style>
