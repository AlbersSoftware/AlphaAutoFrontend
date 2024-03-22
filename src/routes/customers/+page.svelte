<nav>
<ul>
<li style="margin-bottom: 20px;">
<a href="/home">Back to Home</a>
</li>
<li>

<li>
<ul>
</nav>
<h1>Customers page</h1>



  <!-- Search bar -->
  <div class="w-full mb-4">
    <label for="search" class="sr-only">Search</label>
    <div class="relative">
      <input type="text" id="search" name="search" placeholder="Search" class="block w-full px-4 py-2 border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 text-gray-700">
      <div class="absolute inset-y-0 right-0 pr-3 flex items-center pointer-events-none">
        <!-- Heroicon name: solid/search -->
        <svg class="h-5 w-5 text-gray-400" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
          <path fill-rule="evenodd" d="M14.293 13.293a1 1 0 0 1-1.414 1.414l-3.5-3.5a1 1 0 1 1 1.414-1.414l3.5 3.5a1 1 0 0 1 0 1.414z" clip-rule="evenodd" />
          <path fill-rule="evenodd" d="M10 18a8 8 0 1 1 0-16 8 8 0 0 1 0 16zm0-2a6 6 0 1 0 0-12 6 6 0 0 0 0 12z" clip-rule="evenodd" />
        </svg>
      </div>
    </div>
  </div>

<script>
  import { onMount} from 'svelte';
  import axios from 'axios';
  import Swal from 'sweetalert2';

  let customers = [];
  let vehicles = [];
  let selectedRow = null;
  // Fetch customer data from the backend
  onMount(async () => {
    try {
      const response = await axios.get('http://localhost:8081/customers');
      customers = response.data; // Assuming the response contains an array of customers
    } catch (error) {
      console.error('Error fetching customers:', error);
    }
  } );


   async function fetchVehicles(customerId) {
    try {
      const response = await axios.get(`http://localhost:8081/vehicles/customers/${customerId}/vehicles`);
      vehicles = response.data; // Assuming the response contains an array of vehicles for the given customer ID
    } catch (error) {
      console.error(`Error fetching vehicles for customer ID ${customerId}:`, error);
    }
  }

function handleRowClick(event, customer) {
    // Fetch vehicles for the selected customer
    fetchVehicles(customer.id);

    // Handle row click logic
    if (selectedRow) {
      selectedRow.classList.remove('selected-row');
    }
    
    // Add 'selected-row' class to the clicked row
    const clickedRow = event.target.closest('tr');
    clickedRow.classList.add('selected-row');
    
    // Update the selectedRow variable
    selectedRow = clickedRow;
    
    // Log the selected customer
    console.log("Selected customer:", customer);
  }

  async function deleteCustomer() {
    if (!selectedRow) {
      return; // No row selected
  }

  // Get the customer ID from the selected row
  const customerId = selectedRow.querySelector('td:first-child').textContent;

  try {
        // Fetch vehicles for the selected customer
        const response = await axios.get(`http://localhost:8081/vehicles/customers/${customerId}/vehicles`);
        const vehicles = response.data;

        // Check if the selected customer has associated vehicles
        const hasAssociatedVehicles = vehicles.length > 0;

        if (hasAssociatedVehicles) {
            // Display a message indicating that the customer cannot be deleted
            Swal.fire({
                title: 'Error!',
                text: 'Cannot delete customer with associated vehicles. You must delete the vehicles associated with the customer first.',
                icon: 'error',
            });
            return; // Exit the function
        }


    // Ask for confirmation before deleting the customer
    const result = await Swal.fire({
      title: 'Are you sure?',
      text: 'You will not be able to recover this customer!',
      icon: 'warning',
      showCancelButton: true,
      confirmButtonColor: '#3085d6',
      cancelButtonColor: '#d33',
      confirmButtonText: 'Yes, delete it!'
    });

    if (result.isConfirmed) {
      // User confirmed, send request to delete the customer
      const customerId = selectedRow.querySelector('td:first-child').textContent; // Assuming the first cell contains the customer ID
      try {
        await axios.delete(`http://localhost:8081/customers/${customerId}`);
        // Customer deleted successfully
        Swal.fire(
          'Deleted!',
          'Your customer has been deleted.',
          'success'
        );
        // Optionally, update the customers array after deletion
        customers = customers.filter(c => c.id !== customerId);
        // Clear the selected row
        selectedRow = null;
      } catch (error) {
        console.error('Error deleting customer:', error);
        Swal.fire(
          'Error!',
          'Failed to delete the customer.',
          'error'
        );
      }
    }
  }

  catch (error) {
        console.error('Error fetching vehicles for customer:', error);
        Swal.fire(
            'Error!',
            'Failed to fetch vehicles for the customer.',
            'error'
        );
    }
}


</script>





<div class="overflow-x-auto">
  <table class="table-auto min-w-full divide-y divide-gray-200">
    <thead>
      <tr>
        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">ID</th>
        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">First Name</th>
        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Last Name</th>
        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Address</th>
        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">City</th>
        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Province</th>
        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Postal Code</th>
        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Phone Number</th>
        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Email</th>
        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Customer Notes</th>
      </tr>
    </thead>
    <tbody class="bg-white divide-y divide-gray-200">
     {#each customers as customer}
     <tr class="selectable-row" on:click={(event) => handleRowClick(event, customer)} >
        <td class="px-6 py-4 whitespace-nowrap text-gray-700">{customer.id}</td>
        <td class="px-6 py-4 whitespace-nowrap text-gray-700">{customer.first_name}</td>
        <td class="px-6 py-4 whitespace-nowrap text-gray-700">{customer.last_name}</td>
        <td class="px-6 py-4 whitespace-nowrap text-gray-700">{customer.address}</td>
        <td class="px-6 py-4 whitespace-nowrap text-gray-700">{customer.city}</td>
        <td class="px-6 py-4 whitespace-nowrap text-gray-700">{customer.province}</td>
        <td class="px-6 py-4 whitespace-nowrap text-gray-700">{customer.postal_code}</td>
        <td class="px-6 py-4 whitespace-nowrap text-gray-700">{customer.phone_number}</td>
        <td class="px-6 py-4 whitespace-nowrap text-gray-700">{customer.email}</td>
        <td class="px-6 py-4 whitespace-nowrap text-gray-700">{customer.customer_notes}</td>
      </tr>
    {/each}
    </tbody>
  </table>
</div>










<div class="overflow-x-auto">
  <table class="table-auto min-w-full divide-y divide-gray-200">
    <thead>
      <tr>
       
        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Vehicles</th>
      </tr>
    
      
     
   
  </table>

</div>




<div class="overflow-x-auto">
  <table class="table-auto min-w-full divide-y divide-gray-200">
    <thead>
      <tr>
       
        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Vin Number</th>
          <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Brand</th>
            <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Model</th>
             <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Color</th>
             <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Year</th>
              <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Odometer</th>
               <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">License</th>
      </tr>
    </thead>
    <tbody class="bg-white divide-y divide-gray-200">
       {#each vehicles as vehicle}
     <tr>
      
       <td class="px-6 py-4 whitespace-nowrap text-gray-700">{vehicle.vin}</td>
       <td class="px-6 py-4 whitespace-nowrap text-gray-700">{vehicle.make}</td>
        <td class="px-6 py-4 whitespace-nowrap text-gray-700">{vehicle.model}</td>
         <td class="px-6 py-4 whitespace-nowrap text-gray-700">{vehicle.color}</td>
           <td class="px-6 py-4 whitespace-nowrap text-gray-700">{vehicle.year}</td>
             <td class="px-6 py-4 whitespace-nowrap text-gray-700">{vehicle.odometer}</td>
               <td class="px-6 py-4 whitespace-nowrap text-gray-700">{vehicle.license}</td>

      </tr>
     {/each}
     
    </tbody>
  </table>

</div>
 <div style="margin-bottom: 20px;"/>

<div style="margin-bottom: 20px;">
  <button type="button" class="w-full md:w-auto flex justify-center py-6 px-2 md:px-2 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-blue-500 hover:bg-green-600 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500" onclick="window.location.href='/customers'">
    Edit Customer Info
  </button>
</div>

<div style="margin-bottom: 20px;">
  <button type="button" class="w-full md:w-auto flex justify-center py-6 px-2 md:px-2 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-blue-500 hover:bg-green-600 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500" onclick="window.location.href='/customers'">
    Edit Customer Vehicle Info
  </button>
</div>

<div style="margin-bottom: 20px;">
  <button type="button" class="w-full md:w-auto flex justify-center py-6 px-2 md:px-2 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-red-500 hover:bg-red-600 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500" onclick="window.location.href='/customers'">
    Delete Vehicle
  </button>
</div>
  <div style="margin-bottom: 20px;">
  <button type="button" class="w-full md:w-auto flex justify-center py-6 px-2 md:px-2 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-red-500 hover:bg-red-600 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500" on:click={deleteCustomer}>
    Delete Customer
  </button>
  

   <div style="margin-bottom: 80px;"/>
</div>






