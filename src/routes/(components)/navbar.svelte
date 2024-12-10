<script lang="ts">
    // import { wretch } from 'wretch';
    import { goto } from "$app/navigation";
    //@ts-ignore
    import { LogOut } from "lucide-svelte";
    import * as Avatar from "$lib/components/ui/avatar/index.js";
    import { onMount } from "svelte";

    let error: string | null = null;
    let data_username:string;
    const profile = () =>{
        const session = sessionStorage.getItem('userData')
        if(session){
            const username = JSON.parse(session).username
            goto(`/profile/${username}`);
        }
    }
    const logOut = async () => {
        try {
            sessionStorage.removeItem("userData");
            localStorage.removeItem("token");
            await goto("/authorization"); 
        } catch (err) {
            console.error("Logout error:", err);
        }
    };

    onMount(async () => {
        const session = sessionStorage.getItem('userData')
        if(session){
            data_username = JSON.parse(session).username;
        }
    });
</script>

<div class="w-full h-[64px] bg-[#40A2E3] sticky top-0 items-center z-50">
    <div
        class="flex w-full items-center h-full justify-between px-10 shadow-md"
    >
        <img src="/logo-white.png" alt="logo" class="w-8" />
        <div class="flex items-center justify-center gap-8 text-white">
            <h1 class="hover:text-xl duration-300" ><a href="/home">Home</a></h1>
            <h1 class="hover:text-xl duration-300" ><a href="/jobs">Jobs</a></h1>
            <h1 class="hover:text-xl duration-300" ><a href="/chat">Chat</a></h1>
            <button on:click={logOut}><LogOut class="hover:text-red-500 duration-300"/></button>

            <p class="text-red-500">{error}</p>
            <div class="flex justify-center items-center gap-5">
                <h1 class="font-semibold">{data_username}</h1>
                <Avatar.Root>
                    <button on:click={profile}>
                        <Avatar.Image
                            src="https://github.com/shadcn.png"
                            alt="@shadcn"
                        />
                        <Avatar.Fallback>CN</Avatar.Fallback>
                    </button>
                </Avatar.Root>
            </div>
        </div>
    </div>
</div>
