<script lang="ts">
	import '../app.css';
	let { children } = $props();
	//@ts-ignore
	import { Toaster } from "$lib/components/ui/sonner";
</script>
<Toaster />
{@render children()}
