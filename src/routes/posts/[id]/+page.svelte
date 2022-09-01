<script>
    import Navbar from "../../../components/Navbar.svelte";
    import { createClient } from '@supabase/supabase-js';
    import { page } from "$app/stores";
    const supabase = createClient(import.meta.env.VITE_SUPABASE_URL, import.meta.env.VITE_SUPABASE_KEY);
    import { onMount } from "svelte";
    let id = $page.params.id;
    let allData = [];

    onMount(async () => {
        let { data, error } = await supabase.from('posts').select('*').eq('id', id);
        allData = data;
    })
</script>
<Navbar />
{#each allData as data}
    <div class="relative left-1/2 -translate-x-1/2 w-[22rem] sm:w-[28rem] h-44 bg-white drop-shadow-md top-44 md:top-40 sm:top-40">
        <div class="relative left-1/2 -translate-x-1/2 w-16 h-16 bg-blue-600/30 top-3 rounded-full">
            <i class="relative left-1/2 -translate-x-1/2 text-2xl top-4 text-blue-600 fa-solid fa-newspaper"></i>
        </div>
        <center>
            <h2 class="relative inline-block text-md font-extrabold top-6 opacity-70"><i class="fa-solid fa-pencil text-lg"></i> {data.name}</h2>
            <h2 class="relative inline-block text-md font-extrabold top-6 opacity-70 mx-2"><i class="fa-solid fa-clock"></i> {data.date}</h2>
            <h2 class="relative inline-block text-md font-extrabold top-6 opacity-70"><i class="fa-solid fa-hashtag"></i> {data.id}</h2>
            <h2 class="relative text-md font-extrabold top-8 opacity-70"><i class="fa-solid fa-paperclip"></i> {data.description}</h2>
        </center>
    </div>
    <div class="relative left-1/2 -translate-x-1/2 w-[22rem] sm:w-[65rem] h-auto bg-white drop-shadow-md top-52 ">
        <p class="relative text-sm text-gray-600 break-words p-3">{data.contents}</p>
    </div>
{/each}