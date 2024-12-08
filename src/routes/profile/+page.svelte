<script lang="ts">
    import * as Avatar from "$lib/components/ui/avatar/index.js";
    import { Button } from "$lib/components/ui/button/index.js";
    import Postelement from "../home/(component)/postelement.svelte";
    import * as Dialog from "$lib/components/ui/dialog/index.js";
    import { Input } from "$lib/components/ui/input/index.js";

    //@ts-ignore
    import { Send, Book } from "lucide-svelte";

    import wretch from "wretch";
    import { onMount } from "svelte";
    import { goto } from "$app/navigation";

    let userData: { id: number; username: string; fullname: string } | null = null;

    


    onMount(async () => {
        const sessionUserData = sessionStorage.getItem("userData");
        if (sessionUserData) {
            userData = JSON.parse(sessionUserData);
            console.log(userData?.username)
            goto(`/profile/${userData?.username}`);
        }
    });
</script>
