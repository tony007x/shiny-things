<script lang="ts">
	import ChatBox from './(component)/ChatBox.svelte';
	import Navbar from './../(components)/navbar.svelte';
    import Sidebar from './(component)/Sidebar.svelte';
    import ProfilePanel from './(component)/ProfilePanel.svelte';
</script>

<Navbar/>
<div class="flex h-[calc(100vh-64px)] w-full">
    <Sidebar />
    <main class="flex-grow w-1/2 overflow-y-auto p-4">
        <ChatBox/>
        <slot />
        <!-- Dynamic Content -->
    </main>
    <ProfilePanel />
</div>
