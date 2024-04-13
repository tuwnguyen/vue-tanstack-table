<script setup>
import people from '@/mockDataPeople.json'
import cars from '@/mockDataCars.json'
import { format } from 'date-fns'
import TableOriginal from '@/components/TableOriginal.vue'
import TableTanstack from '@/components/TableTanstack.vue'
import EditButton from '@/components/EditButton.vue'
import { h } from 'vue'

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

const columnsCars = [
{
    accessorKey: 'id',
    header: 'ID',
    enableSorting: false,
  },
  {
    accessorKey: 'car_make',
    header: 'Car Make',
  },
  {
    accessorKey: 'car_model',
    header: 'Car Model',
  },
  {
    accessorKey: 'car_year',
    header: 'Year',
  },
  {
    accessorKey: 'price',
    header: 'Price',
  },
]
</script>

<template>
  <div class="container mx-auto px-8 py-8">
    <TableTanstack :data="people" :columns="columnsPeople"/>
    <div class="my-8">
      <TableTanstack :data="cars" :columns="columnsCars"/>
    </div>
  </div>
</template>
