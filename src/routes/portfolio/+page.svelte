<script lang="ts">
    import { toast } from "svelte-sonner";
    import PortfolioBoxPerform from "./(component)/PortfolioBoxPerform.svelte";
    import wretch from "wretch";

    // รับค่าจาก API และแสดงข้อความใน UI
    let text: string = "";
    let message: string = "";

    const send = async () => {
        try {
            const res = await wretch("api/v1/users/test")
                .post({
                    text: text,
                })
                .json((c) => {
                    console.log(c.newtext);
                    message = c.newtext;
                });

            // แสดงข้อความเมื่อสำเร็จ
            toast.success("Message sent successfully!");
        } catch (error) {
            console.error(error);
            toast.error("Failed to send message.");
        }
    };

    interface PortDetail {
        context: string;
        pictureURL: string;
    }

    // ตัวอย่างข้อมูลสำหรับวนลูป
    let portDatailArr: PortDetail[] = [
        {
            context: "Loop 1",
            pictureURL:
                "https://certifier.io/_next/image?url=https%3A%2F%2Fres.cloudinary.com%2Fcertifier%2Fimage%2Fupload%2Fv1709650100%2F51_participation_formal_darkviolet_landscape_ff5e55e81a.png&w=3840&q=100",
        },
        {
            context: "Loop 2",
            pictureURL:
                "https://cdn-ghkoj.nitrocdn.com/kjYfdEBKRwdYwvHQyjaYBdTGFpFGjqYW/assets/images/optimized/rev-891ee2d/sertifier.com/blog/wp-content/uploads/2020/10/certificate-text-samples.jpg",
        },
    ];
</script>

<!-- UI -->
<div class="flex flex-col w-full bg-[#FFF6E9] gap-4 p-5">
    <!-- วนลูป portDatailArr เพื่อแสดง PortfolioBoxPerform -->
    {#each portDatailArr as detailEach}
        <PortfolioBoxPerform detail={detailEach} />
    {/each}

    <!-- ช่องข้อความสำหรับส่งข้อมูล -->
    <div class="mt-4">
        <h1 class="text-lg font-bold">Hello {message}</h1>
        <input
            type="text"
            bind:value={text}
            placeholder="Enter your text"
            class="w-full border p-2 rounded mt-2"
        />
        <button
            on:click={send}
            class="mt-2 bg-blue-500 text-white p-2 rounded hover:bg-blue-600"
        >
            Send
        </button>
    </div>
</div>
