<template>
  <div class="py-4">
  <div class="container">
  <div class="title border-bottom d-flex align-items-center justify-content-between py-2">
    <h5>Task</h5>
    <div class="d-flex align-items-center">
      <label for="category">Select with Category:</label>
      <select class="form-select" aria-label="select" v-model="listCategory" id="category">
        <option value="semua">semua</option> 
        <option>kerjaan</option>
        <option>makanan</option>
        <option>game</option>
      </select>

      <input
      type="text"
      class="form-control"
      placeholder="Search"
      v-model="searchQuery"
      >
      <div class="d-flex align-item-center justify-content-end w-100">
      <span class="me-2">View As</span>
      <button
      class="btn btn-outline-secondary py-1 px-3"
      @click="isGrid=!isGrid" >
      {{ isGrid ? 'Grid' : 'List' }}
      </button>
    </div>
    </div>
  </div>
  <div class="list-task row">
    <CardItem :task="tasks[0]" :isGrid="isGrid" />
    <CardItem :task="tasks[1]" :isGrid="isGrid" />
    <CardItem :task="tasks[2]" :isGrid="isGrid" />
    <CardItem
    v-for="(task, i) in resultQuery"
    :key="i"
    :task="task"
    :isGrid="isGrid"
    />
    <CardItem v-for="(item, index) in chooseCategory" :key="index" :task="item" :isGrid="isGrid"/>
  </div>
  <div class="action py-2">
    <a
      v-if="!isCreating" href="#" class="add-button"
      @click="isCreating=!isCreating">Add Task</a>
    <div v-else class="add-card">
      <div class="card mb-2">
        <div class="card-body d-flex flex-column p-0">
        <input class="form-control border-0 mb-2" placeholder="Title" type="text">
        <textarea
        class="form-control border-0 small" placeholder="Description"
        rows="3"></textarea>
        </div>
      </div>
      <div class="button-wrapper d-flex">
        <button class="btn btn-primary me-2">Save</button>
        <button
        class="btn btn-outline-secondary"
        @click="isCreating =!isCreating">Cancel</button>
      </div>
    </div>
  </div>
  </div>
  </div>
</template>
<script>
import CardItem from "@/components/Card/CardItem.vue"
export default {
  layout (context) {
    return 'custom'
  },
  components: {
    CardItem
  },
  data() {
    return {
      searchQuery: '',
      // Tipe layout daftar task
      isGrid: true,
      // Status saat menambahkan task
      isCreating: false,
      listCategory: ["semua", "kerjaan", "makanan", "game"],
      // Daftar task
      tasks: [
        {
          title: 'Kerjaan 1',
          description: 'semua kerjaan hari ini harus selesai!',
          isDone: false,
          category: 'kerjaan',
        },
        {
          title: 'Makanan enak',
          description: 'buat list makanan enak versi saya',
          isDone: false,
          category: 'makanan',
        },
        {
          title: 'game populer',
          description: 'buat list game populer saat ini',
          isDone: false,
          category: 'game',
        },
      ]
    }
  },
  computed: {
    resultQuery() {
      if (this.searchQuery) {
        return this.tasks.filter((item) => {
          return this.searchQuery
          .toLowerCase()
          .split(" ")
          .every((v) => item.title.toLowerCase().includes(v));
        });
      } else {
        console.log(this.tasks)
        return this.tasks
      }
    },
    chooseCategory() {
      if (this.listCategory == 'kerjaan') {
        return this.tasks.filter((item) => {
          return item.category == 'kerjaan'
        })
      } else if (this.listCategory == 'makanan') {
        return this.tasks.filter((item) => {
          return item.category == 'makanan'
        })
      } else if (this.listCategory == 'game') {
        return this.tasks.filter((item) => {
          return item.category == 'game'
        })
      } else {
        return this.tasks
      }
    }
  }
}
</script>
<style>
</style>