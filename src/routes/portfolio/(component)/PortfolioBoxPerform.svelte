<script lang="ts">
    import * as Dialog from "$lib/components/ui/dialog/index.js";
    import { Button } from "$lib/components/ui/button/index.js";
    import wretch from "wretch";

    // รับ props
    export let detail: { context: string; pictureURL: string };
 
    let context = detail.context;
    let pictureURL = detail.pictureURL;
    let context_edit: string = context; // ดึงค่าเริ่มต้นจาก context
    let NewEdit: string;

    interface PostEdit {
        userID: number;
        postID: number;
        updated_detail: string;
    }

    const boxPost: PostEdit[] = [];

    const updateContext = async () => {
        try {
            const res = await wretch("api/v1/users/updatePortfolioDetail")
                .post({
                    userID: 1,
                    postID: 1,
                    updated_detail: context_edit,
                })
                .json((c) => {
                    NewEdit = c.updated_detail; 

                    boxPost.push({
                        userID: boxPost.length+1,
                        postID: boxPost.length+1,
                        updated_detail: NewEdit,
                    });

                    context_edit = NewEdit;
                    context = NewEdit;
                });

            console.log("Updated boxPost:", boxPost);
        } catch (error) {
            console.error("Error updating context:", error);
        }
    };
</script>

<div class="flex flex-row gap-3 p-5 h-full">
    <!-- Left section -->
    <div
        class="flex flex-col w-3/4 min-h-[250px] border gap-3 p-3 rounded-3xl shadow-md bg-[#D9D9D9]"
    >
        <h1>{context}</h1>

        <!-- Dialog -->
        <Dialog.Root>
            <Dialog.Trigger>Edit</Dialog.Trigger>
            <Dialog.Content>
                <Dialog.Header>
                    <Dialog.Title>Editing Portfolio Context</Dialog.Title>
                    <Dialog.Description>
                        <input
                            type="text"
                            bind:value={context_edit}
                            class="bg-white border rounded p-2"
                        />
                        <Button variant="outline" on:click={updateContext}>
                            Submit
                        </Button>
                    </Dialog.Description>
                </Dialog.Header>
            </Dialog.Content>
        </Dialog.Root>
    </div>

    <!-- Right section -->
    <div
        class="flex flex-col w-1/4 min-h-[250px] border gap-3 p-3 rounded-3xl shadow-md bg-[#D9D9D9]"
    >
        <img src={pictureURL as string} alt="placeholder" class="w-full" />
    </div>
</div>
