<script setup lang="ts">
import { onMounted, ref } from 'vue'

interface User {
  id: number
  fullName: string
  email: string
  startDate: string
  salary: number
  age: number
  status: number
  avatar?: string
}

const headers = [
  { title: 'Name', key: 'fullName' },
  { title: 'Email', key: 'email' },
  { title: 'Date', key: 'startDate' },
  { title: 'Salary', key: 'salary' },
  { title: 'Age', key: 'age' },
  { title: 'Status', key: 'status' },
  { title: 'Actions', key: 'actions' },
]

const statusOptions = [
  { text: 'Current', value: 1 },
  { text: 'Professional', value: 2 },
  { text: 'Rejected', value: 3 },
]

const users = ref<User[]>([])
const editDialog = ref(false)
const deleteDialog = ref(false)
const editedIndex = ref(-1)

const defaultUser: User = {
  id: -1,
  fullName: '',
  email: '',
  startDate: '',
  salary: 0,
  age: 0,
  status: 1,
}

const editedUser = ref<User>({ ...defaultUser })
function resolveStatus(status: number) {
  if (status === 1)
    return { color: 'primary', text: 'Current' }

  if (status === 2)
    return { color: 'success', text: 'Professional' }

  if (status === 3)
    return { color: 'error', text: 'Rejected' }

  return { color: 'grey', text: 'Unknown' }
}

function closeEdit() {
  editDialog.value = false
  editedIndex.value = -1
  editedUser.value = { ...defaultUser }
}

function closeDelete() {
  deleteDialog.value = false
  editedIndex.value = -1
  editedUser.value = { ...defaultUser }
}

function openEditDialog(user: User) {
  editedIndex.value = users.value.findIndex(u => u.id === user.id)
  editedUser.value = { ...user }
  editDialog.value = true
}

function saveUser() {
  if (editedIndex.value > -1) {
    users.value[editedIndex.value] = { ...editedUser.value }
  }
  else {
    editedUser.value.id = Date.now()
    users.value.push({ ...editedUser.value })
  }

  closeEdit()
}

function openDeleteDialog(user: User) {
  editedIndex.value = users.value.findIndex(u => u.id === user.id)
  editedUser.value = { ...user }
  deleteDialog.value = true
}

function deleteUser() {
  users.value.splice(editedIndex.value, 1)
  closeDelete()
}

function getInitials(name: string): string {
  return name
    .split(' ')
    .map(word => word.charAt(0))
    .join('')
    .toUpperCase()
}

onMounted(() => {
  users.value = [
    {
      id: 1,
      fullName: 'Hamas coringuei',
      email: 'essabomba@deuruim.com',
      startDate: '2023-01-01',
      salary: 5000,
      age: 30,
      status: 1,
      avatar: 'https://randomuser.me/api/portraits/women/1.jpg',
    },
    {
      id: 2,
      fullName: 'Ju bonow',
      email: 'seila@gmail.com',
      startDate: '2022-07-15',
      salary: 6200,
      age: 28,
      status: 2,
      avatar: 'https://randomuser.me/api/portraits/women/2.jpg',
    },
    {
      id: 3,
      fullName: 'Acabou',
      email: 'a@ideia.com',
      startDate: '2022-07-15',
      salary: 1200,
      age: 18,
      status: 3,
      avatar: '',
    },
    {
      id: 4,
      fullName: 'Verder',
      email: 'Imperio@gmail.com',
      startDate: '2022-07-15',
      salary: 620,
      age: 29,
      status: 4,
      avatar: '',
    },
    {
      id: 5,
      fullName: 'Hamas coringuei',
      email: 'essabomba@deuruim.com',
      startDate: '2023-01-01',
      salary: 5000,
      age: 30,
      status: 1,
      avatar: '',
    },
    {
      id: 6,
      fullName: 'Ju bonow',
      email: 'seila@gmail.com',
      startDate: '2022-07-15',
      salary: 6200,
      age: 28,
      status: 2,
      avatar: '',
    },
    {
      id: 7,
      fullName: 'Acabou',
      email: 'a@ideia.com',
      startDate: '2022-07-15',
      salary: 1200,
      age: 18,
      status: 3,
      avatar: '',
    },
    {
      id: 8,
      fullName: 'Verder',
      email: 'Imperio@gmail.com',
      startDate: '2022-07-15',
      salary: 620,
      age: 29,
      status: 4,
      avatar: '',
    },
    {
      id: 9,
      fullName: 'Acabou',
      email: 'a@ideia.com',
      startDate: '2022-07-15',
      salary: 1200,
      age: 18,
      status: 3,
      avatar: '',
    },
    {
      id: 10,
      fullName: 'Verder',
      email: 'Imperio@gmail.com',
      startDate: '2022-07-15',
      salary: 620,
      age: 29,
      status: 4,
      avatar: '',
    },
  ]
})
</script>

<template>
  <VDataTable
    :headers="headers"
    :items="users"
    :items-per-page="5"
  >
    <template #item.status="{ item }">
      <VChip :color="resolveStatus(item.status).color">
        {{ resolveStatus(item.status).text }}
      </VChip>
    </template>
    <template #item.actions="{ item }">
      <div class="d-flex gap-2">
        <VBtn icon @click="openEditDialog(item)">
          <VIcon icon="tabler-edit" />
        </VBtn>
        <VBtn icon @click="openDeleteDialog(item)">
          <VIcon icon="tabler-trash" />
        </VBtn>
      </div>
    </template>
    <template #item.fullName="{ item }">
      <div class="d-flex align-center">
        <VAvatar size="36" class="me-3" color="primary" variant="tonal">
          <template v-if="item.avatar">
            <VImg :src="item.avatar" />
          </template>
          <template v-else>
            {{ getInitials(item.fullName) }}
          </template>
        </VAvatar>

        <div class="d-flex flex-column">
          <span class="font-weight-medium text-high-emphasis">
            {{ item.fullName }}
          </span>
          <small class="text-disabled">{{ item.email }}</small>
        </div>
      </div>
    </template>
  </VDataTable>
  <VDialog v-model="editDialog" max-width="500">
    <VCard title="Edit User">
      <VCardText>
        <VTextField v-model="editedUser.fullName" label="Name" />
        <VTextField v-model="editedUser.email" label="Email" />
        <VTextField v-model="editedUser.salary" label="Salary" type="number" />
        <VTextField v-model="editedUser.age" label="Age" type="number" />
        <VTextField v-model="editedUser.startDate" label="Start Date" />
        <VSelect
          v-model="editedUser.status"
          :items="statusOptions"
          item-title="text"
          item-value="value"
          label="Status"
        />
      </VCardText>
      <VCardActions>
        <VSpacer />
        <VBtn @click="closeEdit" color="secondary">Cancel</VBtn>
        <VBtn @click="saveUser" color="primary">Save</VBtn>
      </VCardActions>
    </VCard>
  </VDialog>
  <VDialog v-model="deleteDialog" max-width="400">
    <VCard title="Confirm Delete">
      <VCardText>
        Are you sure you want to delete <strong>{{ editedUser.fullName }}</strong>?
      </VCardText>
      <VCardActions>
        <VSpacer />
        <VBtn @click="closeDelete" color="secondary">Cancel</VBtn>
        <VBtn @click="deleteUser" color="error">Delete</VBtn>
      </VCardActions>
    </VCard>
  </VDialog>
</template>

<style scoped>
.d-flex {
  display: flex;
}

.gap-2 {
  gap: 0.5rem;
}
</style>
