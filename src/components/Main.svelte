<nav>
<ul>
<li>

</li>
<li>

</li>
</ul>
</nav>
<h1>Login page</h1>
 <img src="src/images/alphalogo.png" alt="Logo" class="mx-auto h-44" style="margin-top: 90px;">

 <div class="min-h-screen flex items-center justify-center custom-background">
  <div class="max-w-md w-full space-y-8">
   
    <h1 class="text-3xl font-extrabold text-center text-gray-100">Login</h1>
    <form class="mt-8 space-y-6" action="#" method="POST">
      <div class="rounded-md shadow-sm -space-y-px">
        <div>
          <label for="username" class="sr-only">Username</label>
          <input id="username" name="username" type="text" autocomplete="username" required
    class="appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-black rounded-t-md focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
    placeholder="Username">
        </div>
        <div>
          <label for="password" class="sr-only">Password</label>
          <input id="password" name="password" type="password" autocomplete="current-password" required
            class="appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-b-md focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
            placeholder="Password">
        </div>
      </div>

      <div>
   <div>
  <button type="button" on:click={login} class="w-full flex justify-center py-2 px-4 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-blue-500 hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500">
  Login
</button>

</div>

    </form>
  </div>
</div>



{#if popup}
  <!-- Popup content goes here -->
  <div class="fixed top-0 left-0 w-full h-full flex items-center justify-center bg-black bg-opacity-50">
    <div class="bg-white p-8 rounded shadow-lg text-black">
      <p>Invalid username or password.</p>
      <button class="mt-4 px-4 py-2 bg-black text-white rounded" on:click={() => popup = false}>Close</button>
    </div>
  </div>
{/if}

 <div style="margin-bottom: 80px;"/>






<script>
  let popup = false;

  function login() {
    const username = document.getElementById('username').value;
    const password = document.getElementById('password').value;
 console.log('Username:', username);
    console.log('Password:', password);

    if (username !== 'billy@alphaauto.com' || password !== 'alpha') {
        console.log('Login failed');
      popup = true;
    } else {
         console.log('Login successful. Redirecting to home...');
      // Redirect to home if username and password are correct
      window.location.href = '/home';
    }
  }
</script>
