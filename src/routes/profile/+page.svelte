<script lang="ts">
    import Navbar from "../(components)/navbar.svelte";
    import * as Avatar from "$lib/components/ui/avatar/index.js";
    import { Button } from "$lib/components/ui/button/index.js";
    import Postelement from "../home/(component)/postelement.svelte";
    
    //@ts-ignore
    import { Send, Pen } from "lucide-svelte";
</script>
<div class="flex w-full h-[calc(100vh-64px)] overflow-y-auto bg-[#FFF6E9]">
    <!-- left box -->
    <div class="flex flex-col w-[33.33%] border-r bg-[#BBE2EC] h-full gap-5 p-3">
        <!-- <div class="fixed gap-5 p-3 flex flex-col h-full border-r border-[green]"> -->
        <!--edit profile-->
        <div class="text-[#656565] text-right ">
            edit
        </div>
        <!--profile-->
        <div class="flex flex-col justify-center gap-2">
            <!--pic-->
            <div class="flex justify-center">
                <Avatar.Root class="w-[250px] h-[250px]">
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
            <Button class="text-[#000000] text-[16px] rounded-2xl gap-2 hover:bg-slate-300 w-fit h-fit bg-[#D9D9D9] shadow-md"><Send size={20}/>Send Message</Button>
        </div>
        <!--Other..-->
        <div class="flex flex-col justify-start gap-2">
            <div class="text-[black] text-[20px]">Skill</div>
            <div class="text-[black] text-[20px]">Education</div>
        </div>
        <div class="flex flex-col justify-start gap-2">
            <!--Facebook-->
            <img src="/facebook.png" alt="facebook" class="h-[40px] w-[40px]">
            <!--Github-->
            <img src="/github.png" alt="github" class="h-[40px] w-[40px]">
            <!--X-->
            <img src="/x.png" alt="x" class="h-[40px] w-[40px]">
        </div>
        <div class="flex justify-center">
            <Button class = "text-[18px] text-[white] w-fit h-fit gap-2 bg-[#40A2E3] hover:bg-[#3280b4] rounded-2xl shadow-md"><Pen size={20}/>Edit Profile
            </Button>
        </div>
        <!-- </div> -->
    </div>

    <!-- middle box -->
    <div class="flex flex-col w-full p-5 gap-4 overflow-scroll">
        <Postelement/>
        <Postelement/>
        <Postelement/>
        <Postelement/>
        <Postelement/>
        <Postelement/>
        <Postelement/>
        <Postelement/>
    </div>

    <!-- right box -->
    <div class="flex w-1/2 border-l">
    </div>
</div>