<script lang="ts">
    import * as Avatar from "$lib/components/ui/avatar/index.js";
    import { Button } from "$lib/components/ui/button/index.js";
   
</script>

<div class="flex w-full min-h-[50px] h-fit border items-center justify-between p-2 bg-[#D9D9D9] rounded-2xl shadow-md">
    <div class="flex gap-4" >
        <div class="flex gap-5 items-center pl-3">
            <Avatar.Root class="w-[50px] h-[50px]">
                <Avatar.Image  src="https://github.com/shadcn.png" alt="@shadcn" />
                <Avatar.Fallback>CN</Avatar.Fallback>
            </Avatar.Root>
            
        </div>
        <div class="flex flex-col text-black">
            <h1>Title</h1>
            <p1>Username</p1>
            <p2>Date</p2>
        </div>
    </div>
    <div class="flex py-2 gap-4 p-5 " >
        <Button class="flex px-6 bg-white text-[#16374D] rounded-full py-0 hover:bg-slate-300 gap-1 items-center" >Reject</Button>
        <Button class="flex px-6 bg-[#16374D] text-white rounded-full py-0 hover:bg-slate-300 gap-1 items-center" >Accept</Button>
    </div>
</div>