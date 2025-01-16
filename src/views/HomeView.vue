<template>
  <main>
    <div class="flex">
      <div class="flex w-full relative justify-center flex-col items-center">
        <div
          class="w-full bg-cyan-500 h-[200px] flex justify-center items-center flex-col"
        >
          <!-- generic header area -->
          <div class="max-w-[1280px] w-full flex px-[20px] pt-[40px]">
            <div class="w-[200px]">
              <img src="@/assets/Avatar.png" />
            </div>
            <div class="flex-1 border-black pt-[20px] px-[20px] flex">
              <div class="flex flex-col align-center justify-center">
                <span class="font-bold text-white text-4xl">John Doe</span>
                <span class="text-white text-1xl">Last Online: 2 days ago</span>
              </div>
              <div class="flex justify-center items-center ml-4 space-x-4">
                <button
                  class="flex justify-center items-center h-[55px] w-[200px] border-[1px] rounded border-white text-white hover:bg-white hover:text-cyan-500"
                >
                  <SupportIcon></SupportIcon>
                  <span class="pl-2">Send Message</span>
                </button>
                <button
                  class="flex justify-center items-center h-[55px] w-[200px] border-[1px] rounded border-white text-white hover:bg-white hover:text-cyan-500"
                >
                  <CommunityIcon></CommunityIcon>
                  <span class="pl-2">Send Message</span>
                </button>
              </div>
            </div>
          </div>
        </div>

        <div
          class="max-w-[1280px] w-full flex mt-[50px] px-[20px] flex-col space-y-4 pb-[20px]"
        >
          <div class="flex justify-between">
            <input
              v-model="searchQuery"
              class="border p-2 mb-4 w-full max-w-[300px]"
              placeholder="Type here to filter by name"
            />
            <button
              class="flex justify-center items-center h-[40px] w-[200px] border-[1px] rounded border-cyan-500 text-cyan-500 hover:bg-cyan-500 hover:text-white"
              @click="refreshPage"
            >
              <span class="pl-2">Refresh</span>
            </button>
          </div>

          <div class="flex w-full px-8 space-x-8" @click="handleClick">
            <div class="flex-1 font-thin">Date</div>
            <div class="flex-[2] font-thin">Name</div>
            <div class="flex-1 font-thin">Gender</div>
            <div class="flex-1 font-thin">Country</div>
            <div class="flex-[2] font-thin">Email</div>
          </div>
          <div v-if="loading" class="spinner"></div>
          <template v-else v-for="item in filteredUsers" :key="item.email">
            <PeopleRow
              :date="formatDate(item.dob.date)"
              :name="item.name.first + ' ' + item.name.last"
              :gender="item.gender"
              :country="item.location.country"
              :email="item.email"
              @user-clicked="openDialog"
            ></PeopleRow>
          </template>

          <div class="flex justify-between">
            <button
              class="flex justify-center items-center h-[40px] w-[200px] border-[1px] disabled:text-gray-400 disabled:border-gray-400 disabled:hover:bg-white rounded border-cyan-500 text-cyan-500 hover:bg-cyan-500 hover:text-white"
              @click="prevPage"
              :disabled="page === 1 || loading"
            >
              &lt; Previous Page
            </button>
            <button
              class="flex justify-center items-center h-[40px] w-[200px] border-[1px] rounded disabled:text-gray-400 disabled:border-gray-400 disabled:hover:bg-white border-cyan-500 text-cyan-500 hover:bg-cyan-500 hover:text-white"
              @click="nextPage"
              :disabled="loading"
            >
              Next Page >
            </button>
          </div>
        </div>
      </div>
    </div>
    <PeopleDialog
      v-model:isOpen="isDialogOpen"
      :people-details="compDetails"
    ></PeopleDialog>
  </main>
</template>

<script setup>
import SupportIcon from '../components/icons/IconSupport.vue';
import CommunityIcon from '../components/icons/IconCommunity.vue';
import PeopleRow from '../components/PeopleRow.vue';
import PeopleDialog from '../components/PeopleDialog.vue';
import { ref, onMounted, computed } from 'vue';

onMounted(async () => {
  await fetchRandomUsers();
});

const listOfUsers = ref([]);
const searchQuery = ref('');
const loading = ref(false);
const page = ref(1);

async function fetchRandomUsers() {
  if (loading.value) return;

  try {
    loading.value = true;
    const response = await fetch(
      `https://randomuser.me/api/?page=${page.value}&results=20`
    );
    const data = await response.json();
    listOfUsers.value = data.results;
    loading.value = false;
  } catch (e) {
    console.error(e);
    loading.value = false;
  }
}

const filteredUsers = computed(() => {
  return listOfUsers.value.filter((user) => {
    const fullName = `${user.name.first} ${user.name.last}`.toLowerCase();
    return fullName.includes(searchQuery.value.toLowerCase());
  });
});

const formatDate = (dateString) => {
  const date = new Date(dateString);
  const day = String(date.getDate()).padStart(2, '0');
  const month = String(date.getMonth() + 1).padStart(2, '0');
  const year = date.getFullYear();
  return `${day}-${month}-${year}`;
};

const isDialogOpen = ref(false);

const peopleDetails = ref(null);

const compDetails = computed(() => {
  return peopleDetails.value;
});

const openDialog = (val) => {
  peopleDetails.value = val;
  isDialogOpen.value = true;
};

const nextPage = async () => {
  if (loading.value) return;

  page.value++;
  await fetchRandomUsers();
};

const prevPage = async () => {
  if (loading.value) return;

  page.value--;
  await fetchRandomUsers();
};

const refreshPage = async () => {
  if (loading.value) return;

  page.value = 1;
  await fetchRandomUsers();
};
</script>

<style lang="css" scoped>
.spinner {
  border: 8px solid #f3f3f3;
  border-top: 8px solid #3498db;
  border-radius: 50%;
  width: 50px;
  height: 50px;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>
