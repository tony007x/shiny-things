<script lang="ts">
   import * as Avatar from "$lib/components/ui/avatar/index.js";
   import { Button } from "$lib/components/ui/button/index.js";
   import { Input } from "$lib/components/ui/input/index.js";

   //@ts-ignore
   import { Image , SquarePen } from "lucide-svelte";
</script>

<div class="flex flex-col w-full border h-fit m-4 rounded-xl p-2 bg-[#40A2E3]">
   <div class="flex w-full p-2 gap-5">
      <Avatar.Root>
         <Avatar.Image src="https://github.com/shadcn.png" alt="@shadcn" />
         <Avatar.Fallback>CN</Avatar.Fallback>
      </Avatar.Root>

      <Input placeholder="Share your thought . . ." class="bg-white rounded-2xl" />
   </div>

   <div class="flex w-full gap-4 p-2">
      <Button class="bg-white rounded-2xl text-black gap-2 hover:bg-slate-300"><Image size={20}/>Media</Button>
      <Button class="bg-white rounded-2xl text-black gap-2 hover:bg-slate-300"><SquarePen size={20}/>Write article</Button>
   </div>
</div>
