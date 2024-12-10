<script>
    let user = {
      name: 'Full Name (Receiver)',
      avatar: 'https://via.placeholder.com/150',
    };
  </script>
  
  <div class="flex flex-col items-center p-4 bg-white border-l border-gray-300">
    <!-- <img src={user.avatar} alt="Profile Picture" class="w-24 h-24 rounded-full mb-4" /> -->
    <h3 class="text-lg font-semibold">{user.name}</h3>
    <button class="mt-4 p-2 bg-gray-200 text-gray-800 rounded-lg hover:bg-gray-300">Profile</button>
  </div>
  