<script>
import Table from "./components/Table.vue";
import TableRow from "./components/TableRow.vue";
import TableData from "./components/TableData.vue";
import data from "./data";

export default {
  data() {
    return {
      headers: ["ID", "First Name", "Last Name", "Resume"],
      users: data,
      file: null,
      userId: "",
    };
  },
  components: {
    Table,
    TableRow,
    TableData,
  },
  methods: {
    reset() {
      if (this.file) {
        this.file = null;
        this.$refs[`file${this.userId}`][0].innerHTML = "";
        this.$refs[`tableRow${this.userId}`][0].$el.classList.remove(
          "bg-green-300"
        );
      }
    },
    addRowBackground(userId) {
      if (userId === undefined) {
        userId = this.userId;
      }
      this.$refs[`tableRow${userId}`][0].$el.classList.add("bg-green-300");
    },
    removeRowBackground(userId) {
      if (userId === undefined) {
        userId = this.userId;
      }
      this.$refs[`tableRow${userId}`][0].$el.classList.remove("bg-green-300");
    },
    dragOver(userId, event) {
      this.addRowBackground(userId);
    },
    dragLeave(userId, event) {
      this.removeRowBackground(userId);
    },
    drag(e) {
      // console.log('drag')
    },
    drop(userId, event) {
      this.reset();
      this.userId = userId;
      this.file = event.dataTransfer.files[0];
      this.afterFileSelection();
    },
    doubleClick(userId, event) {
      this.reset();
      this.userId = userId;
      this.addRowBackground();
      let input = document.createElement("input");
      input.type = "file";
      input.onchange = () => {
        let files = Array.from(input.files);
        this.file = files[0];
        console.log(this.file.name);
        this.afterFileSelection();
      };
      input.click();
    },

    afterFileSelection() {
      // Show file details
      this.$refs[`file${this.userId}`][0].innerHTML = this.file.name;
    },

    handleFileUpload() {
      const formData = new FormData();
      formData.append("file", this.file);
      this.$refs[
        `file${this.userId}`
      ][0].innerHTML = `Uploading... ${this.file.name}`;
      // TODO: Do your fetch here
      setTimeout(
        function (thisScope) {
          thisScope.reset();
        },
        2000,
        this
      );
    },
  },
};
</script>

<template>
  <div class="p-4">
    <div class="text-center mb-4">
      <h1 class="text-2xl">Vue3 file upload table</h1>
      <h2 class="text-sm">
        Drag and drop or double click on any table row to upload a file.
      </h2>
    </div>
    <Table :headers="headers">
      <div
        class="table-row-wrapper w-full"
        v-for="user in users"
        :key="user.id"
      >
        <TableRow
          :ref="`tableRow${user.id}`"
          @dragover.prevent="dragOver(user.id, $event)"
          @dragleave.prevent="dragLeave(user.id, $event)"
          @drag.prevent="drag"
          @drop.prevent="drop(user.id, $event)"
          @dblclick.prevent.stop="doubleClick(user.id, $event)"
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
            <button
              @click="handleFileUpload"
              class="bg-gray-200 px-2 py-1 rounded-md"
            >
              Upload
            </button>
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
