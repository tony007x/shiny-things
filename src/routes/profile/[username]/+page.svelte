<script lang="ts">
    import * as Avatar from "$lib/components/ui/avatar/index.js";
    import { Button } from "$lib/components/ui/button/index.js";
    import * as Dialog from "$lib/components/ui/dialog/index.js";
    import { Input } from "$lib/components/ui/input/index.js";

    //@ts-ignore
    import { Send, Book } from "lucide-svelte";

    import wretch from "wretch";
    import { onMount } from "svelte";
    import { page } from "$app/stores";
    import { json } from "@sveltejs/kit";

    let DialogOpen: boolean;
    let inputName: string | null = null;
    let name: string = "";

    const edit = async () => {
        const res = await wretch("api/v1/profile/profile")
            .put({
                name: name,
            })
            .json((c) => {
                name = inputName == null ? name : inputName;
                console.log(name + " from front");
            });

        DialogOpen = false;
    };
    const handleInput = (e: Event) => {
        const target = e.target as HTMLInputElement | null;
        if (target) {
            inputName = target.value;
        }
    };

    interface TypeData {
        id: number;
        username: string;
        fullname: string;
    }
    let profileData: TypeData;
    let username: string = "";
    //Validate Owner Profile
    let isOwn: boolean = true;
    onMount(async () => {
        const pathParts = window.location.pathname.split("/");
        let ownUser = sessionStorage.getItem("userData");
        username = pathParts[pathParts.length - 1];
        console.log(isOwn)
        if (ownUser) {
            ownUser = JSON.parse(ownUser).username
            isOwn = (ownUser === username)? false: true;
        }

        if (username) {
            try {
                const response = await wretch(`/api/v1/profile/${username}`)
                    .get()
                    .json<TypeData>();
                profileData = response;
            } catch (error) {
                console.error("Error fetching profile:", error);
            }
        }
    });
</script>

<div class="flex w-full h-[calc(100vh-64px)] overflow-y-auto bg-[BBE2EC]">
    <!-- left box -->
    <div
        class="flex flex-col w-[33.33%] border-r bg-[#BBE2EC] h-full gap-5 p-3"
    >
        <!-- <div class="fixed gap-5 p-3 flex flex-col h-full border-r border-[green]"> -->
        <!--edit profile-->
        <div class="flex ml-auto">
            <Dialog.Root
                open={DialogOpen}
                onOpenChange={(open) => (DialogOpen = open)}
            >
                <Dialog.Trigger
                    class="border border-[#D9D9D9] items-center px-2 rounded-2xl hover:bg-slate-100"
                >
                    edit
                </Dialog.Trigger>
                <Dialog.Content>
                    <Dialog.Header>Edit Your Profile</Dialog.Header>
                    <Dialog.Description class="flex flex-col w-full">
                        <div>
                            <div class="flex flex-col py-2 gap-2">
                                <div class="">Name</div>
                                <div class="w-full">
                                    <Input
                                        class="w-full h-[32px]"
                                        placeholder="Enter your name"
                                        value={inputName}
                                        on:input={handleInput}
                                    ></Input>
                                </div>
                                <div class="flex justify-center">
                                    <Button
                                        class="text-[16px] text-[white] w-fit h-fit gap-2 bg-[#40A2E3] hover:bg-[#3280b4] rounded-2xl shadow-md"
                                        on:click={edit}>Submit</Button
                                    >
                                </div>
                            </div>
                        </div>
                    </Dialog.Description>
                </Dialog.Content>
            </Dialog.Root>
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
            {#if profileData}
                <div class="text-[32px] text-[#000000] text-center">
                    {profileData.fullname}
                </div>
            {/if}
        </div>
        <!--send message-->
        {#if isOwn}
            <div class="flex justify-center">
                <Button
                    class="text-[#000000] text-[16px] rounded-2xl gap-2 hover:bg-slate-300 w-fit h-fit bg-[#D9D9D9] shadow-md"
                    ><Send size={20} />Send Message</Button
                >
            </div>
        {/if}
        <!--Other..-->
        <div class="flex flex-col justify-start gap-2">
            <div class="text-[black] text-[20px]">Skill</div>
            <div class="text-[black] text-[20px]">Education</div>
        </div>
        <div class="flex flex-col justify-start gap-2">
            <!--Facebook-->
            <img src="/facebook.png" alt="facebook" class="h-[40px] w-[40px]" />
            <!--Github-->
            <img src="/github.png" alt="github" class="h-[40px] w-[40px]" />
            <!--X-->
            <img src="/x.png" alt="x" class="h-[40px] w-[40px]" />
        </div>
        <div class="flex justify-center">
            <Button
                class="text-[18px] text-[white] w-fit h-fit gap-2 bg-[#40A2E3] hover:bg-[#3280b4] rounded-2xl shadow-md"
            >
                <Book size={20} />Portfolio
            </Button>
        </div>
        <!-- </div> -->
    </div>

    <!-- middle box -->
    <div class="flex flex-col w-full p-5 gap-4 overflow-scroll">
        <!-- <Postelement /> -->
    </div>

    <!-- right box -->
    <div class="flex w-1/2 border-l"></div>
</div>
