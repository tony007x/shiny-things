<script lang="ts">
    import Navbar from "../(components)/navbar.svelte";
    import Applicants from "./(component)/applicants.svelte";
    import Padding from "./(component)/padding.svelte";

    //@ts-ignore
    import { ChevronDown } from "lucide-svelte";

    let isJobs: boolean = false;
    let isOffer: boolean = false;
    let isJobpend: boolean = false;
</script>

<Navbar />

<div class="flex w-full h-[200vh] overflow-y-auto bg-[#FFF6E9]">
    <!-- left box -->
    <div class="flex flex-col w-1/4 border-r-2 bg-[#BBE2EC]">
        <div
            class="flex w-full border-b h-20 justify-between items-center px-4"
        >
            <h1>JOB SEEKER</h1>
            <button class="border" on:click={() => (isJobs = !isJobs)}>
                <ChevronDown class="hover:cursor-pointer hover:text-gray-600" />
            </button>
        </div>
        {#if isJobs}
            <div
                class="flex flex-col w-full border-b items-start px-10 py-4 gap-4 bg-[#33415549]"
            >
                <button
                    class={`cursor-pointer ${isOffer ? "underline" : ""}`}
                    on:click={() => {
                        isOffer = !isOffer;
                        isJobpend = false;
                    }}>Offers</button
                >
                <button
                    class={`cursor-pointer ${isJobpend ? "underline" : ""}`}
                    on:click={() => {
                        isJobpend = !isJobpend;
                        isOffer = false;
                    }}>Pending</button
                >
            </div>
        {/if}
    </div>
    <div class="flex flex-col w-full p-5 gap-4 overflow-scroll">
        {#if isOffer}
            <Applicants />
            <Applicants />
            <Applicants />
            <Applicants />
        {/if}
        {#if isJobpend}
            <Padding />
            <Padding />
            <Padding />
            <Padding />
        {/if}
    </div>
</div>
