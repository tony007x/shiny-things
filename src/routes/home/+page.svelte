<script lang="ts">
	import  wretch from 'wretch';
    import { Input } from "$lib/components/ui/input/index.js";
    import Post from "./(component)/createpost.svelte";
    import Postelement from "./(component)/postelement.svelte";
    import { Badge } from "$lib/components/ui/badge/index.js";
    import { onMount } from "svelte";
    import { goto } from '$app/navigation';
    let errorMessage:string = "";

    onMount(async () => {
        const token = localStorage.getItem("token");

        if (token) {
            try {
                const response = await wretch("api/v1/auth/verify")
                    .options({
                        headers: {
                            Authorization: `Bearer ${token}`, // ใช้ options เพื่อส่ง token ใน headers
                        },
                    })
                    .get()
                    .json();

                if (response) {
                    sessionStorage.setItem('userData', JSON.stringify(response));
                } else {
                    errorMessage = "Failed to fetch user data";
                }
            } catch (error) {
                errorMessage = "An error occurred while fetching user data";
                console.error(error);
            }
        } else {
            errorMessage = "No token found. Please log in.";
            goto("/login"); 
        }
    });
</script>


<div class="flex w-full h-[calc(100vh-64px)] bg-[#FFF6E9]">
    <!-- left box -->
    <div class="flex flex-col w-1/3 border-r-2  p-3">
        <Input placeholder="Search. . ."/>
        <div class="grid grid-cols-2 gap-2 w-full p-4 justify-items-center">
            <Badge class="flex justify-center items-center w-fit  bg-gray-500 text-white rounded-lg px-4">Tags</Badge>
            <Badge class="flex justify-center items-center w-fit  bg-gray-500 text-white rounded-lg px-4">Tags</Badge>
            <Badge class="flex justify-center items-center w-fit  bg-gray-500 text-white rounded-lg px-4">Tags</Badge>
            <Badge class="flex justify-center items-center w-fit  bg-gray-500 text-white rounded-lg px-4">Tags</Badge>
          </div>          
    </div>

    <!-- middle box -->
    <div class="flex flex-col w-full p-5 gap-4 overflow-scroll">
        <Postelement/>
    </div>

    <!-- right box -->
    <div class="flex w-3/4 border-l-2">
        <Post/>
    </div>
</div>