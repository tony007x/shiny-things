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
</script>

<div class="flex flex-col w-full bg-[#FFF6E9]">
    <!-- <PortfolioBoxPerform/>
    <PortfolioBoxPerform/>
    <PortfolioBoxPerform/>
    <PortfolioBoxPerform/>
    <PortfolioBoxSkill/> -->
    <h1>Hello {message}</h1>

    <input type="text" bind:value={text} class="bg-white" />
    <button on:click={send}>Send</button>
</div>
