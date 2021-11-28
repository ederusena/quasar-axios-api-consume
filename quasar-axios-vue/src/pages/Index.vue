<template>
  <q-page>
    <div class="col q-pa-md">
      <q-table
      title="Treats"
      :rows="rows"
      style="height: 400px"
      :columns="columns"
      row-key="id"
      class="col"
      virtual-scroll
      v-model:pagination="pagination"
      :rows-per-page-options="[0]"
      selection="multiple"
      v-model:selected="selected"
      :selected-rows-label="getSelectedString"
    >
      <template v-slot:body-cell-action="props">
        <q-td :props="props">
          <q-btn icon="delete" color="negative" size="sm" dense @click="deleteRow(props.row.id)"/>
          <q-btn icon="create" color="primary" size="sm" dense @click="editRow(props.row.id)" class="q-ml-sm"/>
        </q-td>
      </template>
    </q-table>
    {{ selected }}
      <q-table
      grid
      title="Treats"
      :rows="rows"
      style="height: 400px"
      :columns="columns"
      row-key="id"
      class="col"
      virtual-scroll
      v-model:pagination="pagination"
      :rows-per-page-options="[0]"
      selection="multiple"
      v-model:selected="selected"
      :selected-rows-label="getSelectedString"
    />
    </div>
  </q-page>
</template>

<script>
import { ref, defineComponent } from 'vue'
const selected = ref([])

export default defineComponent({
  name: 'PageIndex',
  data () {
    return {
      selected,
      getSelectedString () {
        return selected.value.length === 0 ? '' : `${selected.value.length} record${selected.value.length > 1 ? 's' : ''} selected of ${selected.value.length}`
      },
      columns: [
        {
          name: 'id',
          label: 'ID Post',
          field: row => row.id,
          align: 'left',
          sortable: true
        },
        {
          name: 'title',
          label: 'Title',
          field: row => row.title,
          align: 'left',
          sortable: true
        },
        {
          name: 'action',
          label: 'Action',
          field: row => row.id,
          align: 'center',
          sortable: true
        }
      ],
      rows: [],
      pagination: ref({
        rowsPerPage: 0
      })
    }
  },
  mounted () {
    this.getPosts()
  },
  methods: {
    // method fetch data with axios from api
    getPosts () {
      this.$axios.get('https://jsonplaceholder.typicode.com/posts').then(res => {
        this.rows = res.data
      })
    },
    // method delete row
    deleteRow (id) {
      // remove row from rows
      this.rows = this.rows.filter(row => row.id !== id)
    },
    // method edit row
    editRow (id) {
      // find row by id
      const row = this.rows.find(row => row.id === id)
      // push row to selected
      console.log(row)
    }
  }
})
</script>
