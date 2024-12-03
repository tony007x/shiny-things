<script lang="ts">
    import Navbar from "../(components)/navbar.svelte";
    import * as Avatar from "$lib/components/ui/avatar/index.js";
</script>
<Navbar/>
<div class="flex w-full h-[200vh] overflow-y-auto bg-[#FFF6E9]">
    <!-- left box -->
    <div class="flex flex-col w-1/3 border-r  p-3 bg-[#BBE2EC] gap-4">
        <div class="text-[#656565] font-inter text-right">edit</div>
        <div class="">
            <Avatar.Root class="w-[267px] h-[267px]">
                    <Avatar.Image
                        src="https://github.com/shadcn.png"
                        alt="@shadcn"
                    />
                    <Avatar.Fallback>CN</Avatar.Fallback>
                </Avatar.Root>
        </div>
    </div>

    <!-- middle box -->
    <div class="flex w-3/4 ">
    </div>

    <!-- right box -->
    <div class="flex w-1/2 border-l">
    </div>
</div>