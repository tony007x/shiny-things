<script lang="ts">
    import * as Avatar from "$lib/components/ui/avatar/index.js";
    import { Button } from "$lib/components/ui/button/index.js";
    import * as Dialog from "$lib/components/ui/dialog/index.js";
    import { Input } from "$lib/components/ui/input/index.js";

    //@ts-ignore
    import { Send, Book, PenLine, X } from "lucide-svelte";

    import wretch from "wretch";
    import { onMount } from "svelte";
    import { page } from "$app/stores";
    import { json } from "@sveltejs/kit";
    import { Description } from "formsnap";

    let DialogOpen: boolean;
    let inputName: string | null = null;
    let inputSkill : string | null = null;
    let inputEducation: string | null = null;
    let inputFacebook : string | null = null;
    let inputGithub : string | null = null;
    let inputX : string | null = null;
    let userId : number;
    let removeSkill = false;
    let removeEducation = false;
    let removeFacebook = false;
    let removeGithub = false;
    let removeX = false;
    
    const test = async() =>{
        const res = await wretch("../api/v1/users/see-user").get().json()
    }
    
    const edit = async () => {
        let fullname: string = profileData.fullname;
        let skill: string = profileData.skill;
        let education: string = profileData.education;
        let facebook: string = profileData.facebook;
        let github : string = profileData.github;
        let x : string = profileData.x;

        fullname = (inputName == null || inputName === "") ? fullname : inputName;
        skill = (inputSkill == null || inputSkill === "") ? skill : inputSkill;
        education = (inputEducation == null || inputEducation === "") ? education : inputEducation;
        facebook = (inputFacebook == null || inputFacebook === "") ? facebook : inputFacebook;
        github = (inputGithub == null || inputGithub === "") ? github : inputGithub;
        x = (inputX == null || inputX === "") ? x : inputX;

        if(removeSkill == true){
            skill = "";
            inputSkill = "";
            removeSkill = false;
        }

        if(removeEducation == true){
            education = "";
            inputEducation = "";
            removeEducation = false;
        }

        if(removeFacebook == true){
            facebook = "";
            inputFacebook = "";
            removeFacebook =false;
        }

        if(removeGithub == true){
            github = "";
            inputGithub = "";
            removeGithub = false;
        }

        if(removeX == true){
            x= "";
            inputX="";
            removeX=false;
        }
        
        const res = await wretch("../api/v1/profiles/edit")
            .put({
                id: userId,
                fullname: fullname,
                skill : skill,
                education : education,
                facebook : facebook,
                github : github,
                x : x
            })
            .json((c) => {
                profileData.fullname = fullname;
                profileData.skill = skill;
                profileData.education = education;
                profileData.facebook = facebook;
                profileData.github = github;
                profileData.x = x;
                // console.log(name + " from front");
            });

        DialogOpen = false;
        inputName = "";
        inputSkill = "";
        inputEducation = "";
        inputFacebook = "";
        inputGithub = "";
        inputX = "";
    };

    const handleRemove = (e:Event) =>{
        const target = e.target as HTMLButtonElement | null;
        if(target){
            switch(target.name){
                case "removeSkill":
                    console.log("skill");
                    removeSkill = true;
                    inputSkill = "";
                case "removeEducation":
                console.log("edu");
                    removeEducation = true;
                    inputEducation = "";
                case "removeFacebook" :
                    console.log("F");
                    removeFacebook = true;
                    inputFacebook = "";
                case "removeGithub" :
                    console.log("g");
                    removeGithub = true;
                    inputGithub = "";
                case "removeX":
                    console.log("x");
                    removeX = true;
                    inputX = "";
                default:
                    console.log("default");
            }
        }
    }

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
                        class="border border-[#D9D9D9] items-center px-2 rounded-2xl hover:bg-slate-100 opacity-70"
                    >
                        edit
                    </Dialog.Trigger>
                    <Dialog.Content>
                        <Dialog.Header>Edit Your Profile</Dialog.Header>
                        <Dialog.Description class="flex flex-col w-full">
                            <div>
                                <div class="flex flex-col py-2 gap-2">
                                    <div class="">Name</div>
                                    <div class="flex flex-row w-full gap-1">
                                        <Input name="name"
                                            class="w-full h-[32px]"
                                            placeholder="Enter your name"
                                            value={inputName}
                                            on:input={handleInput}
                                            ></Input>
                                    </div>
                                    <div class="">Skill</div>
                                    <div class="flex flex-row w-full gap-1">
                                        <Input name="skill"
                                            class="w-[92%] h-[32px]"
                                            placeholder="Enter your skill"
                                            value={inputSkill}
                                            on:input={handleInput}
                                        ></Input>
                                        <Button name="removeSkill" class="w-[8%] h-[32px] rounded-2xl shadow-md text-[16px] text-[656565] hover:bg-slate-300 bg-slate-100" on:click={handleRemove}>X</Button>
                                    </div>
                                    <div class="">Education</div>
                                    <div class="flex flex-row w-full gap-1">
                                        <Input name="education"
                                            class="w-[92%] h-[32px]"
                                            placeholder="Enter your education"
                                            value={inputEducation}
                                            on:input={handleInput}
                                        ></Input>
                                        <Button name="removeEducation" class="w-[8%] h-[32px] rounded-2xl shadow-md text-[16px] text-[656565] hover:bg-slate-300 bg-slate-100" on:click={handleRemove}>X</Button>
                                    </div>
                                    <div class="">Facebook</div>
                                    <div class="flex flex-row w-full gap-1">
                                        <Input name="facebook"
                                            class="w-[92%] h-[32px]"
                                            placeholder="Enter your facebook profile link"
                                            value={inputFacebook}
                                            on:input={handleInput}
                                        ></Input>
                                        <Button name="removeFacebook" class="w-[8%] h-[32px] rounded-2xl shadow-md text-[16px] text-[656565] hover:bg-slate-300 bg-slate-100" on:click={handleRemove}>X</Button>
                                    </div>
                                    <div class="">Github</div>
                                    <div class="flex flex-row w-full gap-1">
                                        <Input name="github"
                                            class="w-[92%] h-[32px]"
                                            placeholder="Enter your github link"
                                            value={inputGithub}
                                            on:input={handleInput}
                                            ></Input>
                                            <Button name="removeGithub"class="w-[8%] h-[32px] rounded-2xl shadow-md text-[16px] text-[656565] hover:bg-slate-300 bg-slate-100" on:click={handleRemove}>X</Button>
                                    </div>
                                    <div class="">X</div>
                                    <div class="flex flex-row w-full gap-1">
                                        <Input name="x"
                                            class="w-[92%] h-[32px]"
                                            placeholder="Enter your X profile link"
                                            value={inputX}
                                            on:input={handleInput}
                                        ></Input>
                                        <Button name="removeX"class="w-[8%] h-[32px] rounded-2xl shadow-md text-[16px] text-[656565] hover:bg-slate-300 bg-slate-100" on:click={handleRemove}>X</Button>
                                    </div>
                                    <div class="flex justify-center">
                                        <Button
                                            class="text-[16px] text-[white] w-fit h-fit gap-2 bg-[#40A2E3] hover:bg-[#3280b4] rounded-2xl shadow-md"
                                            on:click={edit}>Submit
                                            </Button>
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
                <Avatar.Root class="w-[250px] h-[250px] relative group overflow-hidden">
                    <Avatar.Image class="w-full h-full object-cover"
                    src="https://github.com/shadcn.png"
                    alt="@shadcn"
                    />
                    <div class="absolute inset-0 flex items-center justify-center opacity-0 group-hover:opacity-100 transition-opacity duration-300 text-[black]">
                        <PenLine Size={60} color='#000000' strokeWidth={2.75} style="stroke: black;"/>
                    </div>
                    <Dialog.Root>
                        <Dialog.Trigger class="z-10 absolute inset-0 bg-white opacity-0 group-hover:opacity-60 transition-opacity duration-300 cursor-pointer"></Dialog.Trigger>
                        <Dialog.Content>
                            <Dialog.Header>Edit your profile image</Dialog.Header>
                            <Dialog.Description>eiei</Dialog.Description>
                        </Dialog.Content>
                    </Dialog.Root>
                    <!-- Pen Component -->
                    <Avatar.Fallback class="absolute inset-0 flex items-center justify-center text-4xl font-bold">CN</Avatar.Fallback>
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
        <div class="flex flex-col justify-start gap-[0.6rem]">
            <!-- <div class="flex flex-row"> -->
                <div class="text-[white] text-[20px] bg-[#40A2E3] rounded-2xl justify-start p-[0.3rem] shadow-md">Skill</div>
            <!-- </div> -->
            {#if profileData?.skill && profileData.skill !== ""}
                <div class="text-[black] text-[18px] p-[0.3rem] bg-slate-100 rounded-2xl">{profileData.skill}</div>
            {:else}
                <div class="min-h-[34px] leading-[1.5] text-[18px] p-[0.3rem] bg-slate-100 rounded-2xl text-opacity-100"></div>
            {/if}
                <div class="text-[white] text-[20px] bg-[#40A2E3] rounded-2xl justify-start p-[0.3rem] shadow-md">Education</div>
            {#if profileData?.education && profileData.education !== ""}
                <div class="text-[black] text-[18px] p-[0.3rem] bg-slate-100  rounded-2xl">{profileData.education}</div>
            {:else}
                <div class="min-h-[34px] leading-[1.5] text-[18px] p-[0.3rem] bg-slate-100 rounded-2xl"></div>
            {/if}
        </div>
        <div class="flex flex-col justify-start gap-2">
            <!--Facebook-->
            <div class = "flex flex-row">
                {#if profileData?.facebook && profileData.facebook !==""}
                <img src="/facebook.png" alt="facebook" class="h-[40px] w-[40px]" />
                <div class="text-[black] text-[18px] py-1 ml-auto">{profileData.facebook}</div>
                {/if}
            </div>
            <!--Github-->
            <div class = "flex flex-row">
                {#if profileData?.github && profileData.github !== ""}
                <img src="/github.png" alt="github" class="h-[40px] w-[40px]" />
                <div class="text-[black] text-[18px] py-1 ml-auto">{profileData.github}</div>
                {/if}
            </div>
            <!--X-->
            <div class = "flex flex-row">
                {#if profileData?.x && profileData.x !== ""}
                <img src="/x.png" alt="x" class="h-[40px] w-[40px]" />
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
