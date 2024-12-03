<script lang="ts">
	import Label from './../../../lib/components/ui/label/label.svelte';
   import * as Avatar from "$lib/components/ui/avatar/index.js";
   import { Button } from "$lib/components/ui/button/index.js";
   import * as Dialog from "$lib/components/ui/dialog/index.js";
   import { Input } from "$lib/components/ui/input/index.js";
   import { Textarea } from "$lib/components/ui/textarea/index.js";

   //@ts-ignore
   import { Image, SquarePen, Settings } from "lucide-svelte";
    import { Checkbox } from '$lib/components/ui/checkbox/index.js';
</script>

<div class="flex flex-col w-full border h-fit m-4 rounded-xl p-2 bg-[#40A2E3]">
   <div class="flex w-full p-2 gap-5">
      <Avatar.Root>
         <Avatar.Image src="https://github.com/shadcn.png" alt="@shadcn" />
         <Avatar.Fallback>CN</Avatar.Fallback>
       </Avatar.Root>
       
       <Dialog.Root>
         <Dialog.Trigger
           class="text-gray-500 bg-white rounded-2xl w-full text-start pl-3 hover:bg-gray-200"
         >
           Share your thought . . .
         </Dialog.Trigger>
         <Dialog.Content>
           <Dialog.Header>
             <Dialog.Description>
               <div class="flex w-full py-3">
                 <div class="flex bg-[#d9d9d97b] p-2 gap-5 rounded-md">
                   <Avatar.Root>
                     <Avatar.Image
                       src="https://github.com/shadcn.png"
                       alt="@shadcn"
                     />
                     <Avatar.Fallback>CN</Avatar.Fallback>
                   </Avatar.Root>
                   <div class="flex flex-col border justify-between">
                     <h1 class="font-bold">Name</h1>
                     <p>Post for Anyone</p>
                   </div>
                   <Settings />
                 </div>
               </div>
             </Dialog.Description>
             <Dialog.Description>
               <Textarea
                 placeholder="Type your message here."
                 class="min-h-[200px] h-[250px]"
               />
               <div class="flex justify-between pt-4 items-center">
                 <Button
                   class="bg-white rounded-2xl text-black gap-2 hover:bg-slate-300"
                 >
                   <Image size={20} /> Media
                 </Button>
                 <div class="flex items-center gap-2">
                   <div class="flex bg-[#d9d9d97b] items-center gap-2 p-2 rounded-lg">
                     <Checkbox />
                     <Label class="text-[12px]">Post for Recruit</Label>
                   </div>
                   <div class="flex bg-[#d9d9d97b] items-center gap-2 p-2 rounded-lg">
                     <Checkbox />
                     <Label class="text-[10px]">Post to Find Job</Label>
                   </div>
                   <Button class="flex p-3 rounded-lg bg-[#40A2E3] ">Post</Button>
                 </div>
               </div>
             </Dialog.Description>
           </Dialog.Header>
         </Dialog.Content>
       </Dialog.Root>
       
   </div>

   <div class="flex w-full gap-4 p-2">
      <Button class="bg-white rounded-2xl text-black gap-2 hover:bg-slate-300"
         ><Image size={20} />Media</Button
      >
      <Button class="bg-white rounded-2xl text-black gap-2 hover:bg-slate-300"
         ><SquarePen size={20} />Write article</Button
      >
   </div>
</div>
