<script lang="ts">
    import wretch from "wretch";
    import * as Avatar from "$lib/components/ui/avatar/index.js";
    import { Badge } from "$lib/components/ui/badge/index.js";
    import { Button } from "$lib/components/ui/button/index.js";
    //@ts-ignore
    import { Settings2, ThumbsUp, MessageCircle } from "lucide-svelte";
    import { onMount } from "svelte";
    import { goto } from "$app/navigation";

    
</script>

<div
    class="flex flex-col w-full min-h-[250px] h-fit border gap-3 p-3 bg-[#40A2E3] rounded-2xl shadow-md"
>
    <div class="flex gap-3 items-center">
        <!-- top -->
        <Avatar.Root>
            <Avatar.Image src="https://github.com/shadcn.png" alt="@shadcn" />
            <Avatar.Fallback>CN</Avatar.Fallback>
        </Avatar.Root>

        <div class="flex w-full border-[red] justify-between">
            <div class="text-white">
                <h1>Name</h1>
                <div class="flex gap-4 items-center">
                    <h2>20 Dec 2024</h2>
                    <Badge
                        class="flex justify-center items-center w-fit h-fit  bg-gray-500 text-white rounded-lg px-4 py-0"
                        >Tags</Badge
                    >
                </div>
            </div>
            <div class="flex flex-col items-end justify-between">
                <Settings2 color="white" />
                <Button
                    class="px-4 h-4 bg-white text-black rounded-full text-[10px] font-bold hover:bg-slate-300"
                    >Apply</Button
                >
            </div>
        </div>
    </div>
    <!-- Middle -->
    <div class="flex border h-full bg-white rounded-md p-3"></div>

    <!-- Bottom -->
    <div class="flex py-2 gap-4">
        <Button
            class="flex px-4 h-6 bg-white text-black rounded-full hover:bg-slate-300 gap-1 items-center"
            ><ThumbsUp size={16} />LIKE</Button
        >
        <Button
            class="flex px-4 h-6 bg-white text-black rounded-full hover:bg-slate-300 gap-1 items-center"
            ><MessageCircle size={16} />COMMENT</Button
        >
    </div>
</div>
