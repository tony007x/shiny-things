<script lang="ts">
    import Navbar from "../(components)/navbar.svelte";
    import * as Avatar from "$lib/components/ui/avatar/index.js";
    import { Button } from "$lib/components/ui/button/index.js";

    //@ts-ignore
    import { Send } from "lucide-svelte";
    import Postelement from "../home/(component)/postelement.svelte";
</script>
<Navbar/>
<div class="flex w-full h-[200vh] overflow-y-auto bg-[#FFF6E9]">
    <!-- left box -->
    <div class="flex flex-col w-1/3 border-r  p-3 bg-[#BBE2EC] gap-4 h-full">
        <!--edit profile-->
        <div class="text-[#656565] text-right">edit</div>
        <!--profile-->
        <div class="flex flex-col justify-center gap-2">
            <!--pic-->
            <div class="flex justify-center">
                <Avatar.Root class="w-[267px] h-[267px]">
                        <Avatar.Image
                            src="https://github.com/shadcn.png"
                            alt="@shadcn"
                        />
                        <Avatar.Fallback>CN</Avatar.Fallback>
                    </Avatar.Root>
            </div>
            <!--name-->
            <div class = "text-[32px] text-[#000000] text-center">Morty Smith</div>
        </div>
        <!--send message-->
        <div class = "flex justify-center">
            <Button class="text-[#000000] text-[22px] rounded-2xl gap-2 hover:bg-slate-300 w-fit h-fit bg-[#D9D9D9] shadow-md"><Send size={20}/>Send Message</Button>
        </div>
        <!--Other..-->
        <div class="flex flex-col justify-start gap-2">
            <div class="text-[black] text-[20px]">Skill</div>
            <div class="text-[black] text-[20px]">Education</div>
        </div>
        <div class="flex flex-col justify-start gap-2">
            <!--Google-->
            <img src="/google.png" alt="facebook" class="h-[51px] w-[51px]">
            <!--Github-->
            <img src="/google.png" alt="github" class="h-[51px] w-[51px]">
            <!--X-->
            <img src="/google.png" alt="x" class="h-[51px] w-[51px]">
        </div>
        <button class = "text-[22px] text-center text-[white] w-full h-[72px] border gap-3 p-3 bg-[#40A2E3] hover:bg-slate-300 rounded-2xl shadow-md">Edit Profile
        </button>
    </div>

    <!-- middle box -->
    <div class="flex flex-col w-full p-5 gap-4 overflow-scroll">
        <Postelement/>
    </div>

    <!-- right box -->
    <div class="flex w-1/2 border-l">
    </div>
</div>