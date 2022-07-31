<script>
import Table from './components/Table.vue';
import TableRow from './components/TableRow.vue';
import TableData from './components/TableData.vue';
import data from './data'

export default {
  data() {
    return {
      headers: ['ID', 'First Name', 'Last Name', 'Resume'],
      users: data,
      file: null,
      tableRow: null,
      userId: "",
    }
  },
  components: {
    Table,
    TableRow,
    TableData
  },
  methods: {
    reset() {
      if(this.file) {
        this.file = null;
        this.$refs[`file${this.userId}`][0].innerHTML = "";
        this.$refs[`tableRow${this.userId}`][0].$el.classList.remove("bg-green-300");
      }
    },
    dragOver(userId, event) {
      this.$refs[`tableRow${userId}`][0].$el.classList.add("bg-green-300");
    },
    dragLeave(userId, event) {
      this.$refs[`tableRow${userId}`][0].$el.classList.remove("bg-green-300");
    },
    drag(e) {
      // console.log('drag')
    },
    drop(userId, event) {
      this.reset();
      this.userId = userId;
      this.tableRow = event.currentTarget;
      this.file = event.dataTransfer.files[0];
      this.afterFileSelection();
    },
    doubleClick(e) {
      // console.log('double click')
      // TODO: 
    },

    afterFileSelection() {
      // Show file details
      this.$refs[`file${this.userId}`][0].innerHTML = this.file.name;
      // this.$refs[`fileInput${this.currentIndex}`][this.currentIndex] = this.file;
    }
  },
}
</script>

<template>
  <div class="p-4">
    <Table :headers="headers">
      <div class="table-row-wrapper w-full"
        v-for="user in users" :key="user.id"
      >

        <TableRow :ref="`tableRow${user.id}`"
          @dragover.prevent="dragOver(user.id, $event)"
          @dragleave.prevent="dragLeave(user.id, $event)"
          @drag.prevent="drag"
          @drop.prevent="drop(user.id, $event)"
          @dblclick.prevent="doubleClick" 
          >
          <TableData>
            {{ user.id }}
          </TableData>
          <TableData>
            {{ user.firstName }}
          </TableData>
          <TableData>
            {{ user.lastName }}
          </TableData>
          <TableData>         
            <!-- File input may go here -->
            <button class="bg-gray-200 px-2 py-1 rounded-md">Upload</button>
            <div :ref="`file${user.id}`" class="ml-2"></div>
          </TableData>
        </TableRow>
      </div>

    </Table>
  </div>
</template>

<style scoped>
  .table-row-wrapper:nth-child(even) {
    background-color: rgba(243, 244, 246);
  }
</style>