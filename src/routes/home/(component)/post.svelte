<script lang="ts">

let text:string;


</script>

<div class="flex w-full">
   <div class="flex w-[200px] h-[100px]">

    <input type="text" class="border w-60" bind:value={text}>

    <button on:click={()=> alert(text)}>OK</button>
   </div>
</div>