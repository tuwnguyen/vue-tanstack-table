<script setup>
import people from '@/mockDataPeople.json'
import {h, ref} from 'vue'
import EditButton from '@/components/EditButton.vue'

import { 
  useVueTable,
  FlexRender,
  getCoreRowModel,
  getPaginationRowModel,
  getSortedRowModel,
} from '@tanstack/vue-table'
import { format } from 'date-fns';

const columnsPeople = [
  {
    accessorKey: 'id',
    header: 'ID',
    enableSorting: false,
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
    enableSorting: false,
  },
]

const data = ref(people)
const sorting = ref([])
const table = useVueTable({
  data: data.value,
  columns: columnsPeople,
  getCoreRowModel: getCoreRowModel(),
  getPaginationRowModel: getPaginationRowModel(),
  getSortedRowModel: getSortedRowModel(),
  state: {
    get sorting() {
      return sorting.value
    }
  },
  onSortingChange: updaterOrValue => {
    sorting.value = typeof updaterOrValue === 'function'
      ? updaterOrValue(sorting.value)
      : updaterOrValue
  },
  initialState: {
    pagination: {
      pageSize: 10
    }
  }
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
                  :class="{'cursor-pointer select-none': header.column.getCanSort()}"
                  @click="header.column.getToggleSortingHandler()?.($event)"
                >
                  <FlexRender
                    :render="header.column.columnDef.header"
                    :props="header.getContext()"
                  >
                  </FlexRender>
                  {{ 
                    // {asc: '⬆️', desc: '⬇️'}[header.column.getIsSorted()]
                    {asc: '↑', desc: '↓'}[header.column.getIsSorted()]
                  }}
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
      <div class="mt-8 text-sm font-medium text-gray-900 dark:text-white">
        Page {{ table.getState().pagination.pageIndex + 1 }} of {{ table.getPageCount() }} - {{ table.getFilteredRowModel().rows.length }}
      </div>
      <div class="mt-8">
        <label for="countries" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Select page size</label>
        <select id="countries" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-16 p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
        @change="e => table.setPageSize(e.target.value)">
          <option >Choose page size</option>
          <option value="5">5</option>
          <option selected value="10">10</option>
          <option value="15">15</option>
          <option value="20">20</option>
        </select>
      </div>
      <div class="space-x-4 mt-8">
        <button 
          class="border border-solid border-indigo-500 rounded px-2 py-2 hover:bg-indigo-500 hover:text-white disabled:opacity-50 disabled:cursor-not-allowed"
          @click="table.setPageIndex(0)"
        >
          First Page
        </button>
        <button 
          class="border border-solid border-indigo-500 rounded px-2 py-2 hover:bg-indigo-500 hover:text-white disabled:opacity-50 disabled:cursor-not-allowed"
          @click="table.setPageIndex(table.getPageCount() - 1)"  
        >
          Last Page
        </button>
        <button 
          class="border border-solid border-indigo-500 rounded px-2 py-2 hover:bg-indigo-500 hover:text-white disabled:opacity-50 disabled:cursor-not-allowed"
          :disabled="!table.getCanPreviousPage()"
          @click="table.previousPage()"  
        >
          Prev Page
        </button>
        <button 
          class="border border-solid border-indigo-500 rounded px-2 py-2 hover:bg-indigo-500 hover:text-white disabled:opacity-50 disabled:cursor-not-allowed"
          :disabled="!table.getCanNextPage()"
          @click="table.nextPage()"  
        >
          Next Page
        </button>
      </div>
    </div>
  </div>
</template>