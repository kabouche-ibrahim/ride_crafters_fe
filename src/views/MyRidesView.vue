<template>

    <div class="flex items-center ml-2 mt-2">
        <img src="@/views/profile-user.png" alt="Profile" class="w-12 h-12 mr-2">
        <button @click="showLogoutConfirmation" class="text-sm text-gray-500">Kabouche ibrahim mohamed ali</button>
    </div>

    <div v-if="showLogoutConfirmationFlag" class="fixed right-0 mt-16 mr-4 top-5">
          <div class="p-4 bg-white border rounded-md shadow-md">
            <p>Logout?</p>
            <div class="mt-4">
              <button @click="logout" class="px-4 py-2 mr-2 text-white bg-red-500 rounded-md hover:bg-red-600 focus:outline-none">
                Confirm
              </button>
              <button @click="cancelLogout" class="px-4 py-2 ml-2 text-white bg-gray-500 rounded-md hover:bg-gray-600 focus:outline-none"> 
                Cancel
              </button>
            </div>
          </div>
    </div>



    <!-- Table with Search Bar -->
    <div class="max-w-full mx-auto mt-24 overflow-y-auto mb-9" style="max-height: 400px;">
      <div class="flex justify-end">
        <input
          v-model="searchQuery.from"
          type="text"
          class="mr-2 p-2 border rounded-md"
          placeholder="From"
        />

        <button @click="performSearch" class="flex items-center px-2 py-2 mr-2 text-sm text-white bg-blue-500 border border-transparent rounded-md shadow-sm hover:bg-blue-600 focus:outline-none">
          <img src="@/views/search.png" alt="Search" class="w-5 h-5 mr-2" style="filter: invert(100%);">
        </button>

        <input
          v-model="searchQuery.to"
          type="text"
          class="mr-2 p-2 border rounded-md"
          placeholder="To"
        />

        <button @click="performSearch" class="flex items-center px-2 py-2 mr-2 text-sm text-white bg-blue-500 border border-transparent rounded-md shadow-sm hover:bg-blue-600 focus:outline-none">
          <img src="@/views/search.png" alt="Search" class="w-5 h-5 mr-2" style="filter: invert(100%);">
        </button>

        <button @click="showFilterPopup" class="flex items-center px-4 py-2 text-sm font-medium text-white bg-blue-500 border border-transparent rounded-md shadow-sm hover:bg-blue-600 focus:outline-none">
          <img src="@/views/filter.png" alt="Filter" class="w-5 h-5 mr-2" style="filter: invert(100%);">
          Filter
        </button>

      </div>
      <!-- Filter Popup -->
      <div v-if="showFilterPopupFlag" class="fixed top-0 left-0 flex items-center justify-center w-full h-full bg-black bg-opacity-50 backdrop-filter backdrop-blur-sm z-50">
        <div class="max-w-md p-8 mx-auto bg-white rounded-md shadow-md">
          <h2 class="mb-4 text-xl font-semibold">Filter Options</h2>
          
          <!-- Filter by Available Seats -->
          <div class="mb-4">
            <label class="block text-sm font-medium text-gray-700">Available Seats</label>
            <div class="flex items-center">
              <input v-model="filterOptions.minSeats" type="number" class="w-16 p-2 border rounded-md" placeholder="Min">
              <span class="mx-2">to</span>
              <input v-model="filterOptions.maxSeats" type="number" class="w-16 p-2 border rounded-md" placeholder="Max">
            </div>
          </div>
          
          <!-- Filter by Time -->
          <div class="mb-4">
            <label class="block text-sm font-medium text-gray-700">Time</label>
            <input v-model="filterOptions.time" type="text" class="w-full p-2 border rounded-md" placeholder="Enter time">
          </div>
          
          <!-- Filter by Date -->
          <div class="mb-4">
            <label class="block text-sm font-medium text-gray-700">Date</label>
            <input v-model="filterOptions.date" type="date" class="w-full p-2 border rounded-md">
          </div>

          <div class="mt-4">
            <button @click="applyFilters" class="px-4 py-2 text-white bg-blue-500 rounded-md hover:bg-blue-600 focus:outline-none">
              Apply Filters
            </button>
            <button @click="cancelFilters" class="px-4 py-2 ml-2 text-white bg-gray-500 rounded-md hover:bg-gray-600 focus:outline-none">
              Cancel
            </button>
          </div>
        </div>
      </div>
      
      
        <table class="w-full mt-4 border border-collapse border-gray-300">
          <thead class="sticky top-0 bg-white">
            <tr>
              <th v-for="(header, index) in tableHeaders" :key="index" class="p-3 text-center bg-gray-200 border-b">
                {{ header }}
              </th>
              <th class="p-3 text-center bg-gray-200 border-b">Actions</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(row, rowIndex) in tableData" :key="rowIndex">
              <td v-for="(cell, cellIndex) in row" :key="cellIndex" class="p-3 text-center border">
                {{ cell }}
              </td>
              <td class="p-3 text-center border">
                <button @click="showForm(row)" class="px-4 py-2 text-white bg-black rounded-md hover:bg-gray-600 focus:outline-none">
                  View Details
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    
        <!-- Form for Selected Ride -->
        <div v-if="showFormFlag" class="fixed top-0 left-0 flex items-center justify-center w-full h-full bg-black bg-opacity-50 backdrop-filter backdrop-blur-sm">
            <div class="max-w-xl p-8 mx-auto bg-white rounded-md shadow-md">
            <h2 class="mb-4 text-xl font-semibold">Driver Information</h2>
            <div>
                <div>
                <p>Driver's Name: John Doe</p>
                <p>Car Name: Sedan</p>
                <p>Car Color: Blue</p>
                <p>Car Year: 2005</p>
                <p>Matricule: ABC123</p>
                </div>
    
                <h3 class="mt-4 text-lg font-semibold">Meeting Place</h3>
                
                

    
                <h3 class="mt-4 text-lg font-semibold">Destination</h3>
                <p>Alger</p>
                <p>Available Seats: 4</p>
    
                <div class="mt-4">
                <button @click="showConfirmation" class="px-4 py-2 text-white bg-green-500 rounded-md hover:bg-green-600 focus:outline-none">
                    Reserve
                </button>
                <button @click="hideForm" class="px-4 py-2 text-white bg-gray-500 rounded-md hover:bg-gray-600 focus:outline-none">
                    Close
                </button>
                </div>
            </div>
            </div>
        </div>
</template>
<script setup>
import { onMounted, ref } from 'vue';
import { useRouter } from 'vue-router';
  
  
  const router = useRouter();
  const searchQuery = ref('');
  const tableHeaders = ['From', 'To', 'Departure Date', 'Time', 'Available Seats'];
  const tableData = ref([]);

  const filterOptions = ref({
    minSeats: null,
    maxSeats: null,
    time: '',
    date: ''
  });

  const showFilterPopupFlag = ref(false);

  const applyFilters = () => {
    // Implement filter logic using filterOptions
    showFilterPopupFlag.value = false;
    console.log('Filters applied:', filterOptions.value); // Check if the filters are being applied
  };

  const cancelFilters = () => {
    // Reset filter options if needed
    filterOptions.value = {
      minSeats: null,
      maxSeats: null,
      time: '',
      date: ''
    };

    showFilterPopupFlag.value = false;
  };

  const performSearch = () => {
    // Implement search logic using searchQuery
    console.log('Performing search:', searchQuery.value); // Check if the search is being performed
  };

  onMounted(() => {
    fetchData();
  });

  const showForm = () => {
    showFormFlag.value = true;
  };
  
  const hideForm = () => {
    showFormFlag.value = false;
  };
  
  const showLogoutConfirmationFlag = ref(false);

  const showLogoutConfirmation = () => {
    showLogoutConfirmationFlag.value = true;
  };

  const cancelLogout = () => {
    showLogoutConfirmationFlag.value = false;
  };

  const logout = () => {
    // Add logic for logout
    showLogoutConfirmationFlag.value = false;
    // Redirect to the logout page or perform logout actions
    router.push('/');
  };



</script>