<script lang="ts">
    import { toast } from "svelte-sonner";
    import PortfolioBoxPerform from "../(component)/PortfolioBoxPerform.svelte";
    import wretch from "wretch";
    import { onMount } from "svelte";
    import { goto } from "$app/navigation";

    // รับค่าจาก API และแสดงข้อความใน UI
    let username: string = "";
    //Validate Owner Profile
    let isOwn: boolean = true;

    onMount(async () => {
        const token = localStorage.getItem("token");

        if (!token) {
            goto("/authorization");
        }
    });

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
</div>
