<script lang="ts">
    import { toast } from "svelte-sonner";
    import PortfolioBoxPerform from "./(component)/PortfolioBoxPerform.svelte";
    import PortfolioBoxSkill from "./(component)/PortfolioBoxSkill.svelte";

    import wretch from "wretch";
    let text: string;
    let message: string;
    const send = async () => {
        const res = await wretch("api/v1/users/test")
            .post({
                text: text,
            })
            .json((c) => {
                console.log(c.newtext);
                message = c.newtext;
            });
    };

    interface portDatail {
        context: string;
        pictureURL: string;
    }

    let portDatailArr: portDatail[] = [
        {
            context: "Loop 1",
            pictureURL: "https://certifier.io/_next/image?url=https%3A%2F%2Fres.cloudinary.com%2Fcertifier%2Fimage%2Fupload%2Fv1709650100%2F51_participation_formal_darkviolet_landscape_ff5e55e81a.png&w=3840&q=100",
        },
        {
            context: "Loop 2",
            pictureURL: "https://cdn-ghkoj.nitrocdn.com/kjYfdEBKRwdYwvHQyjaYBdTGFpFGjqYW/assets/images/optimized/rev-891ee2d/sertifier.com/blog/wp-content/uploads/2020/10/certificate-text-samples.jpg",
        },
        {
            context: "Loop 3",
            pictureURL: "https://certifier.io/_next/image?url=https%3A%2F%2Fres.cloudinary.com%2Fcertifier%2Fimage%2Fupload%2Fv1709650100%2F51_participation_formal_darkviolet_landscape_ff5e55e81a.png&w=3840&q=100",
        },
        {
            context: "Loop 4",
            pictureURL: "https://cdn-ghkoj.nitrocdn.com/kjYfdEBKRwdYwvHQyjaYBdTGFpFGjqYW/assets/images/optimized/rev-891ee2d/sertifier.com/blog/wp-content/uploads/2020/10/certificate-text-samples.jpg",
        },
    ];
</script>

<div class="flex flex-col w-full bg-[#FFF6E9]">
    {#each portDatailArr as detailEach}
        <PortfolioBoxPerform detail = {detailEach}/>
    {/each}
    <!-- <PortfolioBoxPerform/> -->
    <PortfolioBoxSkill/>
    <h1>Hello {message}</h1>

    <input type="text" bind:value={text} class="bg-white" />
    <button on:click={send}>Send</button>
</div>
