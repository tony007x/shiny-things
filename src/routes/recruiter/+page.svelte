<script lang="ts">
    import Navbar from "../(components)/navbar.svelte";
    import Applicants from "./(component)/applicants.svelte";
    import Padding from "./(component)/padding.svelte";
</script>  

<Navbar/>

<div class="flex w-full h-[200vh] overflow-y-auto bg-[#FFF6E9]">
   <!-- left box -->
    <div class="flex w-1/4 border-r-2 bg-[#BBE2EC]">
    
    </div>
    <div class="flex flex-col w-full p-5 gap-4 overflow-scroll">
        <Applicants/>
        <Padding/>
    </div>
</div>
