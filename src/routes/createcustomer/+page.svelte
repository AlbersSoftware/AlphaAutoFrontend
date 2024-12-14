<nav>
<ul>
<li style="margin-bottom: 20px;">
<a href="/home">Back to Home</a>
</li>
<li>

<li>
<ul>
</nav>



<script>
 import axios from 'axios';
 import Swal from 'sweetalert2';
  let formData = {};

  // Function to handle form submission
  async function handleSubmit() {
    try {

      const phoneNumber = formData.phone_number;
    if (!phoneNumber || phoneNumber.length !== 10 || isNaN(phoneNumber)) {
      // Display error message
      Swal.fire({
        icon: 'error',
        title: 'Error',
        text: 'Customer phone number must be 10 digits',
      });
      return; // Exit function if phone number is invalid
    }


      const response = await axios.post('http://localhost:8081/customers', formData);
      console.log('Customer created successfully:', response.data);

      Swal.fire({
        icon: 'success',
        title: 'Success',
        text: 'Customer has been created successfully',
        timer: 3000, // Auto close after 3 seconds
        timerProgressBar: true,
        showConfirmButton: false
      });
     
    } catch (error) {
      console.error('Error creating customer:', error);
      // Optionally, you can display an error message to the user
    }
  }
</script>

<h1>Create Customers page</h1>

<div class="max-w-4xl mx-auto">
  <form class="mt-8 space-y-6">
    <div class="grid grid-cols-1 gap-y-6 gap-x-4 sm:grid-cols-6">
      <!-- First Name -->
      <div class="sm:col-span-3">
        <label for="first_name" class="block text-sm font-medium text-white-700">First Name</label>
        <input type="text" id="first_name" name="first_name" bind:value={formData.first_name} autocomplete="given-name" required class="mt-1 focus:ring-blue-500 focus:border-blue-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md text-black">
      </div>
      <!-- Last Name -->
      <div class="sm:col-span-3">
        <label for="last_name" class="block text-sm font-medium text-white-700">Last Name</label>
        <input type="text" id="last_name" name="last_name" bind:value={formData.last_name} autocomplete="family-name" required class="mt-1 focus:ring-blue-500 focus:border-blue-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md text-black">
      </div>
      <!-- Address -->
      <div class="sm:col-span-6">
        <label for="address" class="block text-sm font-medium text-white-700">Address</label>
        <input type="text" id="address" name="address" bind:value={formData.address} autocomplete="street-address" required class="mt-1 focus:ring-blue-500 focus:border-blue-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md text-black">
      </div>
      <!-- City -->
      <div class="sm:col-span-3">
        <label for="city" class="block text-sm font-medium text-white-700">City</label>
        <input type="text" id="city" name="city" bind:value={formData.city} autocomplete="address-level2" required class="mt-1 focus:ring-blue-500 focus:border-blue-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md text-black">
      </div>
      <!-- Province -->
      <div class="sm:col-span-2">
        <label for="province" class="block text-sm font-medium text-white-700">State</label>
        <input type="text" id="province" name="province" bind:value={formData.province} autocomplete="address-level1" required class="mt-1 focus:ring-blue-500 focus:border-blue-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md text-black">
      </div>
      <!-- Postal Code -->
      <div class="sm:col-span-1">
        <label for="postal_code" class="block text-sm font-medium text-white-700">Postal Code</label>
        <input type="text" id="postal_code" name="postal_code" bind:value={formData.postal_code} autocomplete="postal-code" required class="mt-1 focus:ring-blue-500 focus:border-blue-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md text-black">
      </div>
      <!-- Phone Number -->
      <div class="sm:col-span-3">
        <label for="phone_number" class="block text-sm font-medium text-white-700">Phone Number</label>
        <input type="text" id="phone_number" name="phone_number" bind:value={formData.phone_number} autocomplete="tel" required class="mt-1 focus:ring-blue-500 focus:border-blue-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md text-black">
      </div>
      <!-- Email Address -->
      <div class="sm:col-span-3">
        <label for="email" class="block text-sm font-medium text-white-700">Email Address</label>
        <input type="text" id="email" name="email" bind:value={formData.email} autocomplete="email" required class="mt-1 focus:ring-blue-500 focus:border-blue-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md text-black">
      </div>

        <div class="sm:col-span-3">
        <label for="customer_notes" class="block text-sm font-medium text-white-700">Customer Notes</label>
        <input type="text" id="customer_notes" name="customer_notes" bind:value={formData.customer_notes} required class="mt-1 focus:ring-blue-500 focus:border-blue-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md text-black">
      </div>
    </div>
    <!-- Add Vehicle Button -->
    <div class="flex justify-center">
      <button type="button" class="inline-flex items-center px-4 py-2 border border-transparent text-sm font-medium rounded-md shadow-sm text-white bg-blue-500 hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500">
        Add Vehicle
      </button>
    </div>


 <!--  Submit button-->
    <div class="flex justify-center">
      <button type="button" class="inline-flex items-center px-4 py-2 border border-transparent text-sm font-medium rounded-md shadow-sm text-white bg-green-500 hover:bg-green-600 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500"on:click={handleSubmit}>
        Submit
      </button>
    </div>

  </form>
</div>

   
  