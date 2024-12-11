<script lang="ts">
    import { Description } from "formsnap";
    import wretch from "wretch";
    import * as Avatar from "$lib/components/ui/avatar/index.js";
    import { Badge } from "$lib/components/ui/badge/index.js";
    import { Button } from "$lib/components/ui/button/index.js";
    import * as Drawer from "$lib/components/ui/drawer/index.js";
    import * as Sheet from "$lib/components/ui/sheet/index.js";
    import { Textarea } from "$lib/components/ui/textarea/index.js";
    import { Input } from "$lib/components/ui/input/index.js";
    import { Label } from "$lib/components/ui/label/index.js";
    import { Select } from "bits-ui";

    //@ts-ignore
    import { Settings, ThumbsUp, MessageCircle, ChevronDown } from "lucide-svelte";
    import { onMount } from "svelte";
    import { goto } from "$app/navigation";
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

    //For Offer/Apply
    let content: string;

    const applyoffer = async (post_id: number, post_type: string) => {
        console.log("sender id: ", profileData.id);
        console.log("post_id: ", post_id);
        console.log("post_type: ", post_type);
        console.log("content: ", content);

        const response: { message: string } = await wretch(
            "api/v1/manage/offerapply",
        )
            .post({
                sender_id: profileData.id,
                post_id: post_id,
                content: content,
                post_type: post_type,
            })
            .json();

        if (response) {
            toast.success(response.message);
            DrawerOpen = false;
        }
    };
    //edit-post
    let newTitle: string;
    let newContent: string;
    let visibility: any = true;

    const postvisible = [
        { value: true, label: "Public" },
        { value: false, label: "Private" },
    ];

    const editPost = async (post_id: number,post_title: string,post_content: string,visibility_post: boolean) => {
        console.log(post_id);
        console.log(newTitle ?? post_title);
        console.log(newContent ?? post_content);
        console.log(visibility);
    };

    const delelePost = async(post_id: number)=>{
        console.log(post_id);
    }

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
        const infoUser = await sessionStorage.getItem("userData");
        if (infoUser) {
            profileData = await JSON.parse(infoUser);
        }
        try {
            const response: Post[] = await wretch("api/v1/posts/get-post")
                .get()
                .json<Post[]>();

                postsStorage = response.filter(post => post.is_visible);
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
                    {#if data.user_id == profileData.id}
                        <Sheet.Root>
                            <Sheet.Trigger asChild let:builder>
                                <Button builders={[builder]} variant="link"
                                    ><Settings
                                        color="white"
                                        class="hover:stroke-gray-300"
                                    />
                                </Button>
                            </Sheet.Trigger>
                            <Sheet.Content side="right">
                                <Sheet.Header>
                                    <Sheet.Title>Edit Post</Sheet.Title>
                                    <Sheet.Description>
                                        Make changes to your post here. Click
                                        save when you're done.
                                    </Sheet.Description>
                                </Sheet.Header>
                                <div class="flex flex-col gap-4 mt-4">
                                    <Label class="font-bold">Visibility</Label>
                                    <Select.Root
                                        onSelectedChange={(s) => {
                                            visibility = s?.value;
                                        }}
                                    >
                                        <Select.Trigger
                                            class="flex w-[180px] text-left bg-gray-100 rounded-lg px-3 justify-between items-center"
                                        >
                                            <Select.Value
                                                placeholder={data.is_visible? "Public" : "Private"}
                                            />
                                            <ChevronDown />
                                        </Select.Trigger>
                                        <Select.Content
                                            class="bg-gray-100 border border-gray-300 rounded-lg"
                                        >
                                            <Select.Group>
                                                {#each postvisible as post}
                                                    <Select.Item
                                                        class="hover:cursor-pointer hover:bg-gray-300 px-4 py-2"
                                                        value={post.value}
                                                    >
                                                        {post.label}
                                                    </Select.Item>
                                                {/each}
                                            </Select.Group>
                                        </Select.Content>
                                    </Select.Root>
                                </div>
                                <div class="flex flex-col gap-2 mt-4">
                                    <div class="flex flex-col gap-2">
                                        <Label class="font-bold"
                                            >Title</Label
                                        >
                                        <Input
                                            bind:value={newTitle}
                                            placeholder={data.title}
                                            class=" h-auto w-full flex"
                                        />
                                    </div>
                                </div>
                                <div class="flex flex-col gap-2 mt-4">
                                    <div class="flex flex-col gap-2">
                                        <Label class="font-bold"
                                            >Descriptions</Label
                                        >
                                        <Textarea
                                            bind:value={newContent}
                                            placeholder={data.content}
                                            class="min-h-[150px] max-h-[300px] h-auto w-full flex"
                                        />
                                    </div>
                                </div>
                                <Sheet.Footer>
                                    <Sheet.Close asChild let:builder>
                                        <div>
                                            <Button type="submit" class="bg-red-500 hover:bg-red-300" on:click={()=>delelePost(data.id)}>
                                                Delete
                                            </Button>
                                            <Button
                                                builders={[builder]}
                                                type="submit"
                                                class="mt-3"
                                                on:click={() =>
                                                    editPost(
                                                        data.id,
                                                        data.title,
                                                        data.content,
                                                        visibility
                                                    )}>Save changes</Button>
                                        </div>
                                    </Sheet.Close>
                                </Sheet.Footer>
                            </Sheet.Content>
                        </Sheet.Root>
                    {/if}
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
                                    <Button
                                        on:click={() =>
                                            applyoffer(
                                                data.id,
                                                data.post_type_name,
                                            )}
                                    >
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
