<script setup>
import { generateClient } from 'aws-amplify/data';
import { ref, onMounted } from 'vue';
import Swal from 'sweetalert2';
import moment from 'moment';

const client = generateClient();
let names = ref([]);

function formatDate(dateString) {
  return moment(dateString).format('DD/MM/YYYY HH:mm');
}

async function addName() {
  const { value: name } = await Swal.fire({
    title: 'Enter your name',
    input: 'text',
    inputPlaceholder: 'Enter your name here...',
    showCancelButton: true,
    confirmButtonText: 'Add Name',
    cancelButtonText: 'Cancel',
    inputValidator: (value) => {
      if (!value) {
        return 'Nice try, but you know I cannot let you do it.';
      }
    },
    customClass: {
      confirmButton: 'swal-button-white-text',
      cancelButton: 'swal-button-white-text',
    },
  });

  if (name) {
    await client.models.Visitors.create({
      signature: name,
    });
    Swal.fire({
      text: `Thanks for your visit, ${name}!`,
      icon: 'success',
      toast: true,
      showConfirmButton: false,
      timer: 2000,
      timerProgressBar: true,
      position: 'center',
      customClass: {
        toast: 'swal-toast-white-text',
      },
    });
    fetchNames();
  }
}

async function fetchNames() {
  const { data: items, errors } = await client.models.Visitors.list();
  names.value = items;

  if (errors) {
    Swal.fire({
      title: 'Error!',
      text: 'There was an issue fetching names.',
      icon: 'error',
      confirmButtonText: 'Try Again',
      customClass: {
        confirmButton: 'swal-button-white-text',
      },
    });
  }
}

async function removeNames() {
  for (const index in names.value) {
    await client.models.Visitors.delete({
      id: names.value[index].id,
    });
  }

  Swal.fire({
    text: `All users signatures have been deleted.`,
    icon: 'success',
    toast: true,
    showConfirmButton: false,
    timer: 2000,
    timerProgressBar: true,
    customClass: {
      toast: 'swal-toast-white-text',
    },
  });

  fetchNames();
}

onMounted(async () => {
  await fetchNames();
});
</script>

<template>
  <div>
    <v-container fluid d-flex>
      <v-row>
        <v-col align="center" justify="center">
          <h1>AWS Learning Experience</h1>
          <h3 class="mt-6">Thanks for visiting this website. Please feel free to log your visit by adding your name to the list below</h3>
          <v-col class="mt-6">
            <span>{{ names.length }}</span>
            <v-btn outlined class="mr-2" width="130px" @click="addName"> Add name </v-btn>
            <v-btn outlined class="ml-2" width="130px" @click="removeNames"> Remove all </v-btn>
            <v-table class="mt-10">
              <thead>
                <tr>
                  <th class="text-left">Name</th>
                  <th class="text-left">Signed at</th>
                </tr>
              </thead>
              <tbody v-if="names.length > 0">
                <tr v-for="name in names" :key="name.id">
                  <td>{{ name.signature }}</td>
                  <td>{{ formatDate(name.createdAt) }}</td>
                </tr>
              </tbody>
              <tbody v-else>
                <tr>
                  <td>No visitors found.</td>
                </tr>
              </tbody>
            </v-table>
          </v-col>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<style>
h1,
h3,
th {
  color: white !important;
}

.v-table {
  width: 500px;
  background-color: rgba(255, 255, 255, 0.062) !important;
  color: white !important;
  outline: 1px solid white !important;
  border-radius: 10px !important;
  font-weight: bold !important;
}

.swal-button-white-text {
  color: white !important;
}

.swal-toast-white-text {
  color: white !important;
}
</style>
