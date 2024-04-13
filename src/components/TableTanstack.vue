<script setup>
import people from '@/mockDataPeople.json'
import {h, ref} from 'vue'
import EditButton from '@/components/EditButton.vue'

import { 
  useVueTable,
  FlexRender,
  getCoreRowModel,
} from '@tanstack/vue-table'
import { format } from 'date-fns';

const columnsPeople = [
  {
    accessorKey: 'id',
    header: 'ID',
  },
  {
    accessorKey: 'first_name',
    header: 'First Name',
  },
  {
    accessorKey: 'last_name',
    header: 'Last Name',
  },
  {
    accessorFn: row => `${row.first_name} ${row.last_name}`,
    header: 'Name',
  },
  {
    accessorKey: 'email',
    header: 'Email',
  },
  {
    accessorKey: 'title',
    header: 'Title',
  },
  {
    accessorKey: 'role',
    header: 'Role',
  },
  {
    accessorKey: 'created_at',
    header: 'Created',
    cell: info => format(new Date(info.getValue()), 'MMM d, yyyy'),
  },
  {
    accessorKey: 'edit',
    header: ' ',
    cell: ({row}) => h(EditButton, {id: row.original.id}),
  },
]

const data = ref(people)
const table = useVueTable({
  data: data.value,
  columns: columnsPeople,
  getCoreRowModel: getCoreRowModel()
})

</script>

<template>
  <div class="px-4 sm:px-6 lg:px-8">
    <div class="mt-8 flow-root">
      <div class="-mx-4 -my-2 overflow-x-auto sm:-mx-6 lg:-mx-8">
        <div class="inline-block min-w-full py-2 align-middle sm:px-6 lg:px-8">
          <table class="min-w-full divide-y divide-gray-300">
            <thead>
              <tr 
                v-for="headerGroup
                in table.getHeaderGroups()"
                :key="headerGroup.id"
              >
                <th 
                  v-for="header
                  in headerGroup.headers"
                  :key="header.id"
                  class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900"
                >
                  <FlexRender
                    :render="header.column.columnDef.header"
                    :props="header.getContext()"
                  >
                  </FlexRender>
                </th>
              </tr>
            </thead>
            <tbody class="divide-y divide-gray-200">

            </tbody>
            <tbody class="divide-y divide-gray-200">
              
              <tr
                v-for="row in table.getRowModel().rows"
                :key="row.id"
              >
                <td
                  v-for="cell in row.getVisibleCells()"
                  :key="cell.id"
                  class="whitespace-nowrap px-3 py-4 text-sm text-gray-500"
                >
                  <FlexRender :render="cell.column.columnDef.cell" :props="cell.getContext()"/>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>