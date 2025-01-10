<script lang="ts">
    import wretch from "wretch";
    import * as Avatar from "$lib/components/ui/avatar/index.js";
    import { Button } from "$lib/components/ui/button/index.js";
    import { onMount } from "svelte";
    import { toast } from "svelte-sonner";

    interface TypeData {
        id: number;
        username: string;
        title: string;
        action_type: string;
        createDate : string;
        content :string;
        status: string;
    }
    let applicantsData : TypeData;
   
    const updateStatus = async (status: string) => {
        if (!applicantsData) return;

        console.log("Updating status ", applicantsData.id);
        console.log("New status:", status);

        try {
            const response: { message: string } = await wretch(
                `api/v1/work/update-status/${applicantsData.id}`
            )
                .post({ status })
                .json();

            if (response) {
                toast.success(response.message);
                applicantsData.status = status;
                console.log("Status updated successfully to:", status);
            }
        } catch (err) {
            console.error("Error updating status:", err);
        }
    };

    onMount(async () => {
        try {
            const response: TypeData = await wretch("api/v1/manage/get-offerApply")
                .get()
                .json<TypeData>();
            if (response){
                applicantsData = response;
                console.log("Fetched applicants data:", applicantsData);
            }
        } catch (err) {
            err = "Failed to load applicant data.";
            console.error(err);
        }
    });

</script>

{#if applicantsData}
    <div class="flex w-full min-h-[50px] h-fit border items-center justify-between p-2 bg-[#D9D9D9] rounded-2xl shadow-md">
        <div class="flex gap-4" >
            <div class="flex gap-5 items-center pl-3">
                <Avatar.Root class="w-[50px] h-[50px]">
                    <Avatar.Image  src="https://github.com/shadcn.png" alt="@shadcn" />
                    <Avatar.Fallback>CN</Avatar.Fallback>
                </Avatar.Root>
                
            </div>
            <div class="flex flex-col text-black">
                <h1>{applicantsData.title}</h1>
                <p>{applicantsData.username}</p>
                <p>{applicantsData.createDate}</p>
            </div>
        </div>
        <div class="flex py-2 gap-4 p-5 ">
            <Button 
                class="flex px-6 bg-white text-[#16374D] rounded-full py-0 hover:bg-slate-300 gap-1 items-center"
                    on:click ={() => updateStatus("Rejected")}
                    >Reject</Button>
            <Button 
                class="flex px-6 bg-[#16374D] text-white rounded-full py-0 hover:bg-slate-300 gap-1 items-center"
                    on:click ={() => updateStatus("Accepted")}
                >Accept</Button> 

        </div>
    </div>
    {:else}
    <p>No applicants</p>
{/if}
