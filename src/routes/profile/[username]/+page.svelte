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
    let inputSkill : string | null = null;
    let inputEducation: string | null = null;
    let inputFacebook : string | null = null;
    let inputGithub : string | null = null;
    let inputX : string | null = null;
    let userId : number;
    let fullname: string = "";
    let skill: string = "";
    let education: string = "";
    let facebook: string = "";
    let github : string = "";
    let x : string = "";

    const test = async() =>{
        const res = await wretch("../api/v1/users/see-user").get().json()
    }

    const edit = async () => {
        fullname = inputName == null ? fullname : inputName;
        skill = inputSkill == null ? skill : inputSkill;
        education = inputEducation == null ? education : inputEducation;
        facebook = inputFacebook == null ? facebook : inputFacebook;
        github = inputGithub == null ? github : inputGithub;
        x = inputX == null ? x : inputX;
        
        const res = await wretch("../api/v1/profiles/edit")
            .put({
                id:userId,
                fullname: fullname,
                skill : skill,
                education : education,
                facebook : facebook,
                github : github,
                x : x
            })
            .json((c) => {
                profileData.fullname = inputName == null ? profileData.fullname : inputName;
                profileData.skill = inputSkill == null ? profileData.skill : inputSkill;
                profileData.education = inputEducation == null ? profileData.education : inputEducation;
                profileData.facebook = inputFacebook == null ? profileData.facebook : inputFacebook;
                profileData.github = inputGithub == null ? profileData.github : inputGithub;
                profileData.x = inputX == null ? profileData.x : inputX;
                // console.log(name + " from front");
            });

        DialogOpen = false;
    };

    const handleInput = (e: Event) => {
        const target = e.target as HTMLInputElement | null;
        if (target) {
            switch (target.name) {
            case "name":
                inputName = target.value;
                break;
            case "skill":
                inputSkill = target.value;
                break;
            case "education":
                inputEducation = target.value;
                break;
            case "facebook":
                inputFacebook = target.value;
                break;
            case "github":
                inputGithub = target.value;
                break;
            case "x":
                inputX = target.value;
                break;
        }
        }
    };

    interface TypeData {
        id: number;
        username: string;
        fullname: string;
        skill : string;
        education : string;
        facebook : string;
        github : string;
        x : string;
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
            // console.log(JSON.parse(ownUser))
            // console.log(typeof(ownUser))
            userId = JSON.parse(ownUser).id
            ownUser = JSON.parse(ownUser).username
            isOwn = (ownUser === username)? false: true;
        }

        if (username) {
            try {
                const response = await wretch(`/api/v1/profiles/${username}`)
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
    <div class="flex flex-col w-[33.33%] border-r bg-[#BBE2EC] h-full gap-5 p-3">
        <!-- <div class="fixed gap-5 p-3 flex flex-col h-full border-r border-[green]"> -->
        <!--edit profile-->
        <div class="flex ml-auto">
            {#if !isOwn}
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
                                    <div class="w-full gap-1">
                                        <Input name="name"
                                            class="w-full h-[32px]"
                                            placeholder="Enter your name"
                                            value={inputName}
                                            on:input={handleInput}
                                            ></Input>
                                    </div>
                                    <div class="">Skill</div>
                                    <div class="w-full">
                                        <Input name="skill"
                                            class="w-full h-[32px]"
                                            placeholder="Enter your skill"
                                            value={inputSkill}
                                            on:input={handleInput}
                                        ></Input>
                                    </div>
                                    <div class="">Education</div>
                                    <div class="w-full">
                                        <Input name="education"
                                            class="w-full h-[32px]"
                                            placeholder="Enter your education"
                                            value={inputEducation}
                                            on:input={handleInput}
                                        ></Input>
                                    </div>
                                    <div class="">Facebook</div>
                                    <div class="w-full">
                                        <Input name="facebook"
                                            class="w-full h-[32px]"
                                            placeholder="Enter your facebook"
                                            value={inputFacebook}
                                            on:input={handleInput}
                                        ></Input>
                                    </div>
                                    <div class="">Github</div>
                                    <div class="w-full">
                                        <Input name="github"
                                            class="w-full h-[32px]"
                                            placeholder="Enter your github"
                                            value={inputGithub}
                                            on:input={handleInput}
                                        ></Input>
                                    </div>
                                    <div class="">X</div>
                                    <div class="w-full">
                                        <Input name="x"
                                            class="w-full h-[32px]"
                                            placeholder="Enter your X"
                                            value={inputX}
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
            {/if}
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
            {#if profileData}
            <div class="text-[black] text-[18px]">{profileData.skill}</div>
            {/if}
            <div class="text-[black] text-[20px]">Education</div>
            {#if profileData}
            <div class="text-[black] text-[18px]">{profileData.education}</div>
            {/if}
        </div>
        <div class="flex flex-col justify-start gap-2">
            <!--Facebook-->
            <div class = "flex flex-row">
                <img src="/facebook.png" alt="facebook" class="h-[40px] w-[40px]" />
                {#if profileData}
                <div class="text-[black] text-[18px] py-1 ml-auto">{profileData.facebook}</div>
                {/if}
            </div>
            <!--Github-->
            <div class = "flex flex-row">
                <img src="/github.png" alt="github" class="h-[40px] w-[40px]" />
                {#if profileData}
                <div class="text-[black] text-[18px] py-1 ml-auto">{profileData.github}</div>
                {/if}
            </div>
            <!--X-->
            <div class = "flex flex-row">
                <img src="/x.png" alt="x" class="h-[40px] w-[40px]" />
                {#if profileData}
                <div class="text-[black] text-[18px] py-1 ml-auto">{profileData.x}</div>
                {/if}
            </div>
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
