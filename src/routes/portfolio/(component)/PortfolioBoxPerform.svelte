<script lang="ts">
    import * as Dialog from "$lib/components/ui/dialog/index.js";
    import { Button } from "$lib/components/ui/button/index.js";
    import { Root } from "$lib/components/ui/accordion/index.js";
    import wretch from "wretch";
    
    let { detail } = $props();
    let context = detail.context;
    let pictureURL = detail.pictureURL;
    let context_edit: string;

    const updateContext = async () => {
        const res = await wretch("api/v1/users/updatePortfolioDetail")
            .post({
                userID: 1, 
                portID: 1,
                updated_detail: context_edit
            })
            .json((c) => {
                console.log(c.updated_detail);
                context = c.updated_detail;
            });
    };
</script>

<div class="flex flex-row gap-3 p-5 h-full">
    <div class="flex flex-col w-3/4 min-h-[250px] border  gap-3 p-3 rounded-3xl shadow-md bg-[#D9D9D9]">
        <p>{context}</p>
        
        <Dialog.Root>
            <Dialog.Trigger>Edit</Dialog.Trigger>
            <Dialog.Content>
              <Dialog.Header>
                <Dialog.Title>Editing Portfolio context</Dialog.Title>
                <Dialog.Description>
                    <input type="text" bind:value={context_edit} class="bg-white" />
                    <Button variant="outline" on:click={updateContext}>Button</Button>
                </Dialog.Description>
              </Dialog.Header>
            </Dialog.Content>
          </Dialog.Root>
    </div>
    <div class="flex flex-col w-1/4 min-h-[250px] border  gap-3 p-3 rounded-3xl shadow-md bg-[#D9D9D9]">
        <img src={pictureURL as string} alt="placeholder" class="w-full">
    </div>
</div>

<!-- <div class="flex flex-row gap-3 p-5 h-full">
    left text box
    <div class="flex flex-col w-3/4 min-h-[250px] border  gap-3 p-3 rounded-3xl shadow-md bg-[#D9D9D9]">
        <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Nemo numquam, 
            velit repellat animi recusandae fugiat maxime enim quae veritatis 
            soluta libero maiores odit aliquid voluptatum fugit non deserunt 
            nihil aspernatur?</p>
    </div>
    right picture box
    <div class="flex flex-col w-1/4 min-h-[250px] border  gap-3 p-3 rounded-3xl shadow-md bg-[#D9D9D9]">
        <img src="https://certifier.io/_next/image?url=https%3A%2F%2Fres.cloudinary.com%2Fcertifier%2Fimage%2Fupload%2Fv1709650100%2F51_participation_formal_darkviolet_landscape_ff5e55e81a.png&w=3840&q=100" alt="placeholder" class="w-full">
        <img src="https://cdn-ghkoj.nitrocdn.com/kjYfdEBKRwdYwvHQyjaYBdTGFpFGjqYW/assets/images/optimized/rev-891ee2d/sertifier.com/blog/wp-content/uploads/2020/10/certificate-text-samples.jpg" alt="placeholder" class="w-full">
    </div>
</div> -->