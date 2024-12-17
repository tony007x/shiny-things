<script lang="ts">
    import Navbar from "../(components)/navbar.svelte";
    import Applicants from "./(component)/applicants.svelte";
    import Padding from "./(component)/padding.svelte";

    //@ts-ignore
    import { ChevronDown } from "lucide-svelte";

    // Job Group States
    let isJobs: boolean = false;
    let isOffer: boolean = false;
    let isJobPending: boolean = false;

    // Recruiter Group States
    let isRecruiter: boolean = false;
    let isApplicant: boolean = false;
    let isRecPending: boolean = false;

    // Reset all states except the active group
    const resetGroup = (group: "job" | "recruiter")=> {
        if (group === "job") {
            isRecruiter = false;
            isApplicant = false;
            isRecPending = false;
        } else if (group === "recruiter") {
            isJobs = false;
            isOffer = false;
            isJobPending = false;
        }
    }
</script>

<Navbar />

<div class="flex w-full h-[calc(100vh-64px)] overflow-y-auto bg-[#FFF6E9]">
    <!-- Left Box -->
    <div class="flex flex-col w-1/4 border-r-2 bg-[#BBE2EC]">
        <!-- Job Seeker Group -->
        <div
            class="flex w-full border-b h-20 justify-between items-center px-4"
        >
            <h1>JOB SEEKER</h1>
            <button
                on:click={() => {
                    isJobs = !isJobs;
                    resetGroup("job");
                }}
            >
                <ChevronDown
                    class={`transition-transform ${
                        isJobs ? "rotate-180" : ""
                    } hover:cursor-pointer hover:text-gray-600`}
                />
            </button>
        </div>
        {#if isJobs}
            <div
                class={`flex flex-col w-full items-start px-10 py-4 gap-4 bg-[#33415549] transition-all duration-500 ${
                    isJobs ? "max-h-96 opacity-100" : "max-h-0 opacity-0"
                } overflow-hidden`}
            >
                <button
                    class={`cursor-pointer ${
                        isOffer ? "underline" : ""
                    } hover:underline`}
                    on:click={() => {
                        isOffer = true;
                        isJobPending = false;
                    }}
                >
                    Offers
                </button>
                <button
                    class={`cursor-pointer ${
                        isJobPending ? "underline" : ""
                    } hover:underline`}
                    on:click={() => {
                        isJobPending = true;
                        isOffer = false;
                    }}
                >
                    Pending
                </button>
            </div>
        {/if}

        <!-- Recruiter Group -->
        <div
            class="flex w-full border-b h-20 justify-between items-center px-4"
        >
            <h1>RECRUITER</h1>
            <button
                on:click={() => {
                    isRecruiter = !isRecruiter;
                    resetGroup("recruiter");
                }}
            >
                <ChevronDown
                    class={`transition-transform ${
                        isRecruiter ? "rotate-180" : ""
                    } hover:cursor-pointer hover:text-gray-600`}
                />
            </button>
        </div>
        <div
            class={`flex flex-col w-full items-start px-10 py-4 gap-4 bg-[#33415549] transition-all duration-500 ${
                isRecruiter ? "max-h-96 opacity-100" : "max-h-0 opacity-0"
            } overflow-hidden`}
        >
            <button
                class={`cursor-pointer ${
                    isApplicant ? "underline" : ""
                } hover:underline`}
                on:click={() => {
                    isApplicant = true;
                    isRecPending = false;
                }}
            >
                Applicants
            </button>
            <button
                class={`cursor-pointer ${
                    isRecPending ? "underline" : ""
                } hover:underline`}
                on:click={() => {
                    isRecPending = true;
                    isApplicant = false;
                }}
            >
                Pending
            </button>
        </div>
    </div>

    <!-- Right Box -->
    <div class="flex flex-col w-full p-5 gap-4 overflow-scroll">
        <!-- Job Seeker Content -->
        {#if isOffer}
            <Applicants />
        {/if}
        {#if isJobPending}
            <Padding />
            <Padding />
            <Padding />
            <Padding />
        {/if}

        <!-- Recruiter Content -->
        {#if isApplicant}
            <Applicants />
        {/if}
        {#if isRecPending}
            <Padding />
            <Padding />
            <Padding />
            <Padding />
        {/if}
    </div>
</div>
