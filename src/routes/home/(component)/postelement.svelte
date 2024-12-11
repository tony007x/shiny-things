<script lang="ts">
    import wretch from "wretch";
    import * as Avatar from "$lib/components/ui/avatar/index.js";
    import { Badge } from "$lib/components/ui/badge/index.js";
    import { Button } from "$lib/components/ui/button/index.js";
    import * as Drawer from "$lib/components/ui/drawer/index.js";

    //@ts-ignore
    import { Settings2, ThumbsUp, MessageCircle } from "lucide-svelte";
    import { onMount } from "svelte";
    import { goto } from "$app/navigation";
    import { Textarea } from "$lib/components/ui/textarea/index.js";
    import { Input } from "$lib/components/ui/input/index.js";
    import { Label } from "$lib/components/ui/label/index.js";
    import { toast } from "svelte-sonner";


    //drawer
    let DrawerOpen: boolean;
    //getUserdata
    interface TypeData {
        id: number;
        username: string;
        fullname: string;
    }
    let profileData: TypeData;
    const infoUser = sessionStorage.getItem("userData");
    if (infoUser) {
        profileData = JSON.parse(infoUser);
    }

    //For Offer/Apply
    let sender: string;
    let content: string;

    const applyoffer = async (post_id:number, post_type: string) => {
        console.log("sender id: ", profileData.id)
        console.log("post_id: ", post_id)
        console.log("post_type: ", post_type)
        console.log("content: ", content);

        const response: {message: string} = await wretch('api/v1/manage/offerapply')
        .post({
            sender_id: profileData.id,
            post_id: post_id,
            content: content,
            post_type: post_type
        }).json()

        if(response){
            toast.success(response.message)
            DrawerOpen = false;
        }
    };

    interface Post {
        id: number;
        user_id: number;
        username: string;
        fullname: string;
        title: string;
        content: string;
        post_type_name: string;
        post_tag_name: string;
        like_count: number;
        is_visible: boolean;
        created_at: string;
    }

    let postsStorage: Post[] = [];

    onMount(async () => {
        try {
            const response: Post[] = await wretch("api/v1/posts/get-post")
                .get()
                .json<Post[]>();

            postsStorage = response;
        } catch (error) {
            console.error("Error loading posts:", error);
        }
    });
</script>

{#each postsStorage as data}
    <div
        class="flex flex-col w-full min-h-[250px] h-fit border gap-3 p-3 bg-[#40A2E3] rounded-2xl shadow-md"
    >
        <div class="flex gap-3 items-center">
            <!-- Top -->
            <button
                on:click={() => {
                    goto(`/profile/${data.username}`);
                }}
            >
                <Avatar.Root>
                    <Avatar.Image
                        src="https://github.com/shadcn.png"
                        alt="@shadcn"
                    />
                    <Avatar.Fallback
                        >{data.fullname[0]}{data.fullname[1]}</Avatar.Fallback
                    >
                </Avatar.Root>
            </button>
            <div class="flex w-full justify-between">
                <div class="text-white">
                    <h1>{data.fullname}</h1>
                    <div class="flex gap-4 items-center">
                        <h2>{data.created_at}</h2>
                        <Badge
                            class="flex justify-center items-center w-fit h-fit bg-gray-500 text-white rounded-lg px-4 py-0"
                            >{data.post_tag_name}</Badge
                        >
                    </div>
                </div>
                <div class="flex flex-col items-end justify-between">
                    <Settings2 color="white" />
                    <Drawer.Root open={DrawerOpen}>
                        {#if data.post_type_name !== "general"}
                            <Drawer.Trigger
                                class="px-4 h-4 bg-white text-black rounded-full text-[10px] font-bold hover:bg-slate-300"
                            >
                                {#if data.post_type_name == "seeker"}
                                    OFFER
                                {:else}
                                    APPLY
                                {/if}
                            </Drawer.Trigger>
                        {/if}
                        <Drawer.Content
                            class="flex items-center justify-center bg-transparent border border-transparent duration-500"
                        >
                            <div
                                class="w-[90%] max-w-[500px] h-fit bg-white p-4 rounded-lg shadow-lg overflow-auto"
                            >
                                <Drawer.Header>
                                    <Drawer.Title
                                        class="flex w-full justify-center"
                                    >
                                        {data.title}
                                    </Drawer.Title>
                                    <Drawer.Description
                                        class="flex w-full justify-center"
                                    >
                                        {data.content}
                                    </Drawer.Description>
                                    <div class=" flex flex-col w-full h-full">
                                        <Textarea
                                            placeholder="Type your message here."
                                            class="min-h-[150px] max-h-[300px] h-auto w-full border border-gray-300 rounded-md p-3 mt-3 mb-3 resize-none"
                                            bind:value={content}
                                        />
                                        <div class="flex flex-col gap-2">
                                            <Label>Upload for file</Label>
                                            <Input
                                                type="file"
                                                class="flex items-center file:text-black file:pt-[3px] hover:bg-gray-200"
                                            />
                                        </div>
                                    </div>
                                </Drawer.Header>
                                <Drawer.Footer
                                    class="flex justify-end space-x-4"
                                >
                                    <Button on:click={()=> applyoffer(data.id, data.post_type_name)}>
                                        {#if data.post_type_name == "seeker"}
                                            OFFER
                                        {:else}
                                            APPLY
                                        {/if}
                                    </Button>
                                    <Drawer.Close>Cancel</Drawer.Close>
                                </Drawer.Footer>
                            </div>
                        </Drawer.Content>
                    </Drawer.Root>
                </div>
            </div>
        </div>
        <!-- Middle -->
        <div class="flex flex-col border h-full bg-white rounded-md p-3">
            <h1 class="font-bold">{data.title}</h1>
            <p>{data.content}</p>
        </div>

        <!-- Bottom -->
        <div class="flex py-2 gap-4">
            <Button
                class="flex px-4 h-6 bg-white text-black rounded-full hover:bg-slate-300 gap-1 items-center"
                ><ThumbsUp size={16} />{data.like_count} LIKE</Button
            >
            <Button
                class="flex px-4 h-6 bg-white text-black rounded-full hover:bg-slate-300 gap-1 items-center"
                ><MessageCircle size={16} />COMMENT</Button
            >
        </div>
    </div>
{/each}
