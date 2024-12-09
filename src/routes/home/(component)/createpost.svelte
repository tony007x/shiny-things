<script lang="ts">
	import { json } from '@sveltejs/kit';
	import  wretch  from 'wretch';
  import Label from "./../../../lib/components/ui/label/label.svelte";
  import * as Avatar from "$lib/components/ui/avatar/index.js";
  import { Button } from "$lib/components/ui/button/index.js";
  import * as Dialog from "$lib/components/ui/dialog/index.js";
  import { Input } from "$lib/components/ui/input/index.js";
  import { Textarea } from "$lib/components/ui/textarea/index.js";
  import { Checkbox } from "$lib/components/ui/checkbox/index.js";
  import { Select } from "bits-ui";
  import { onMount } from "svelte";

  //@ts-ignore
  import { Image, ChevronDown } from "lucide-svelte";
    import { boolean } from "zod";

  const postvisible = [
    { value: true, label: "Public" },
    { value: false, label: "Private" },
  ];

  const postTags = [
    { value: 1, label: "Jobs" },
    { value: 2, label: "Interviews" },
    { value: 3, label: "Resumes" },
    { value: 4, label: "Career" },
    { value: 5, label: "Remote" },
    { value: 6, label: "Freelance" },
    { value: 7, label: "Skills" },
    { value: 8, label: "Growth" },
    { value: 9, label: "Balance" },
    { value: 10, label: "Motivate" },
  ];

  interface TypeData {
    id: number;
    username: string;
    fullname: string;
  }

  let profileData: TypeData | undefined;
  const infoUser =
    typeof window !== "undefined" ? sessionStorage.getItem("userData") : null;

  if (infoUser) {
    profileData = JSON.parse(infoUser);
  }

  // Data bindings
  let username: string = "";
  let title: string = "";
  let content: string = "";
  let visibility: any = true;
  let tags:any = postTags[0].value; // Default to first tag
  let type: number | null = null;
  let DialogOpen: boolean;

  const create = async () => {
    console.log("userid:", profileData?.id);
    console.log("title: ", title);
    console.log("Content:", content);
    console.log("Visibility:", visibility);
    console.log("tags: ", tags);
    console.log("type:", type);
    
    const respone = await wretch('api/v1/posts/create')
    .post({
      user_id: profileData?.id,
      title: title,
      content: content,
      post_type_id: type,
      post_tag_id: tags,
      is_visible: visibility,
      like_count: 0
    })
    .json()
    DialogOpen = false;
  };

  const toggleCheckbox = (option: number) => {
    type = type === option ? null : option;
  };
</script>

<div class="flex flex-col w-full border h-fit m-4 rounded-xl p-2 bg-[#40A2E3]">
  <div class="flex w-full p-2 gap-5">
    <Avatar.Root>
      <Avatar.Image src="https://github.com/shadcn.png" alt="@shadcn" />
      <Avatar.Fallback>CN</Avatar.Fallback>
    </Avatar.Root>

    <Dialog.Root open={DialogOpen} onOpenChange={(open) => (DialogOpen = open)}>
      <Dialog.Trigger
        class="text-gray-500 bg-white rounded-2xl w-full text-left pl-4 py-2 hover:bg-gray-200"
      >
        Share your thought . . .
      </Dialog.Trigger>

      <Dialog.Content
        class="flex w-full max-w-[600px] border border-gray-300 rounded-lg justify-center"
      >
        <Dialog.Header>
          <Dialog.Description>
            <div class="flex w-full">
              <div
                class="flex bg-[#d9d9d97b] p-2 gap-4 rounded-md w-fit items-center px-4"
              >
                <Avatar.Root>
                  <Avatar.Image
                    src="https://github.com/shadcn.png"
                    alt="@shadcn"
                  />
                  <Avatar.Fallback>CN</Avatar.Fallback>
                </Avatar.Root>

                <div class="flex flex-col justify-between w-full">
                  <h1 class="font-bold text-lg">{profileData?.fullname}</h1>
                  <Select.Root
                  onSelectedChange={(s)=>{
                    visibility = s?.value;
                  }}
                  >
                    <Select.Trigger
                      class="flex w-[180px] text-left bg-gray-100 rounded-lg px-3 justify-between items-center"
                    >
                      <Select.Value placeholder="Public" />
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
              </div>
            </div>
          </Dialog.Description>

          <Dialog.Description>
            <Input placeholder="Title" bind:value={title} />
            <Textarea
              placeholder="Type your message here."
              class="min-h-[150px] max-h-[300px] h-auto w-full border border-gray-300 rounded-md p-3 mt-3 mb-3 resize-none"
              bind:value={content}
            />
            <Select.Root
              onSelectedChange={(s)=>{
                tags = s?.value;
              }}
            >
              <Select.Trigger
                class="flex w-[180px] text-left bg-gray-100 rounded-lg px-3 justify-between items-center"
              >
                <Select.Value placeholder={postTags[0].label} />
                <ChevronDown />
              </Select.Trigger>
              <Select.Content
                class="bg-gray-100 border border-gray-300 rounded-lg"
              >
                <Select.Group>
                  {#each postTags as tags}
                    <Select.Item
                      class="hover:cursor-pointer hover:bg-gray-300 px-4 py-2"
                      value={tags.value}
                      label={tags.label}
                    >
                      {tags.label}
                    </Select.Item>
                  {/each}
                </Select.Group>
              </Select.Content>
            </Select.Root>

            <div class="flex justify-between pt-4 items-center gap-4">
              <Button
                class="bg-white text-black rounded-lg gap-2 hover:bg-slate-300 px-4 py-2"
              >
                <Image size={20} /> Media
              </Button>

              <div class="flex items-center gap-4">
                <div
                  class="flex bg-[#d9d9d97b] items-center gap-2 p-2 rounded-lg"
                >
                  <Checkbox
                    checked={type === 1}
                    on:click={() => toggleCheckbox(1)}
                  />
                  <Label class="text-xs">General</Label>
                </div>

                <div
                  class="flex bg-[#d9d9d97b] items-center gap-2 p-2 rounded-lg"
                >
                  <Checkbox
                    checked={type === 2}
                    on:click={() => toggleCheckbox(2)}
                  />
                  <Label class="text-xs">Post for Recruit</Label>
                </div>

                <div
                  class="flex bg-[#d9d9d97b] items-center gap-2 p-2 rounded-lg"
                >
                  <Checkbox
                    checked={type === 3}
                    on:click={() => toggleCheckbox(3)}
                  />
                  <Label class="text-xs">Post to Find Job</Label>
                </div>
              </div>
            </div>

            <Button
              class="flex p-3 rounded-lg bg-[#40A2E3] text-white mt-4 w-full justify-center"
              on:click={create}
            >
              Post
            </Button>
          </Dialog.Description>
        </Dialog.Header>
      </Dialog.Content>
    </Dialog.Root>
  </div>
</div>
