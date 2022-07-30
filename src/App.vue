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
    }
  },
  components: {
    Table,
    TableRow,
    TableData
}
}
</script>

<template>
  <div class="p-4">
    <Table :headers="headers">
      <TableRow v-for="user in users" :key="user.id">
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
          {{ user.resume }}
        </TableData>
      </TableRow>
    </Table>
  </div>
</template>

<style scoped>
  tr:nth-child(even) {
    background-color: rgba(243, 244, 246);
  }
</style>