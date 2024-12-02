<script lang="ts">
    //@ts-ignore
    import { LogOut } from "lucide-svelte";
    import * as Avatar from "$lib/components/ui/avatar/index.js";
</script>

<div class="w-full h-[64px] bg-[#40A2E3] sticky top-0 items-center">
    <div
        class="flex w-full items-center h-full justify-between px-14 shadow-md"
    >
        <img src="/logo-white.png" alt="logo" class="w-8" />
        <div class="flex items-center gap-8 text-white">
            <h1><a href="#Home">Home</a></h1>
            <h1><a href="#Jobs">Jobs</a></h1>
            <h1><a href="#Chat">Chat</a></h1>
            <h1><a href="#logout"><LogOut /></a></h1>
            <Avatar.Root>
                <Avatar.Image
                    src="https://github.com/shadcn.png"
                    alt="@shadcn"
                />
                <Avatar.Fallback>CN</Avatar.Fallback>
            </Avatar.Root>
        </div>
    </div>
</div>
