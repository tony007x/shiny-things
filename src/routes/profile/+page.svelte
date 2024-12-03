<script lang="ts">
    import Navbar from "../(components)/navbar.svelte";
    import * as Avatar from "$lib/components/ui/avatar/index.js";
    import { Button } from "$lib/components/ui/button/index.js";

    //@ts-ignore
    import { Send } from "lucide-svelte";
</script>
<Navbar/>
<div class="flex w-full h-[200vh] overflow-y-auto bg-[#FFF6E9]">
    <!-- left box -->
    <div class="flex flex-col w-1/3 border-r  p-3 bg-[#BBE2EC] gap-4">
        <div class="text-[#656565] font-inter text-right border border-[red]">edit</div>
        <div class="flex flex-col justify-center border border-[red] gap-2">
            <div class="flex justify-center">
                <Avatar.Root class="w-[267px] h-[267px]">
                        <Avatar.Image
                            src="https://github.com/shadcn.png"
                            alt="@shadcn"
                        />
                        <Avatar.Fallback>CN</Avatar.Fallback>
                    </Avatar.Root>
            </div>
            <div class = "text-[32px] text-[#000000] font-inter text-center">Morty Smith</div>
        </div>
        <div class = "flex justify-center border border-[red]">
            <Button class="text-[#000000] font-inter text-[22px] rounded-2xl gap-2 hover:bg-slate-300 w-[267px] h-[61px] bg-[#D9D9D9]"><Send size={20}/>Send Message</Button>
        </div>
    </div>

    <!-- middle box -->
    <div class="flex w-3/4 ">
    </div>

    <!-- right box -->
    <div class="flex w-1/2 border-l">
    </div>
</div>