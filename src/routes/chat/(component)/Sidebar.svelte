<script lang="ts">
    import { goto } from "$app/navigation";

    let Chats: {
        id: string;
        name: string;
        lastMessage: string;
        time: string;
    }[] = [
        { id: "1", name: "User 1", lastMessage: "Hello!", time: "10:41" },
        { id: "2", name: "User 2", lastMessage: "How are you?", time: "09:30" },
    ];
</script>

<div class="w-64 bg-gray-100 p-4 overflow-y-auto">
    <input
        type="text"
        placeholder="Search Messenger"
        class="w-full p-2 mb-4 border border-gray-300 rounded-lg"
    />
    <div class="space-y-4">
        {#each Chats as chat}
            <button
                class="flex items-center space-x-4 p-2 cursor-pointer hover:bg-gray-200 rounded-lg"
                on:click={() => (goto(`/chat/${chat.id}`) )}
            >
                <img
                    src="https://via.placeholder.com/40"
                    alt="Avatar"
                    class="w-10 h-10 rounded-full"
                />
                <div class="flex-grow">
                    <strong class="block text-gray-800">{chat.name}</strong>
                    <p class="text-sm text-gray-600">{chat.lastMessage}</p>
                    <small class="text-xs text-gray-500">{chat.time}</small>
                </div>
            </button>
        {/each}
    </div>
</div>
