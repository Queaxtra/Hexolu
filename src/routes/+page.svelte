<script>
    import Navbar from "../components/Navbar.svelte";
    import { createClient } from '@supabase/supabase-js';
    import { onMount } from "svelte";
    const supabase = createClient(import.meta.env.VITE_SUPABASE_URL, import.meta.env.VITE_SUPABASE_KEY);
    const user = supabase.auth.user()

    let allData = [];

    onMount(async () => {
        await supabase.from("posts").select().then(res => {
            allData = res.data
        })
    })
</script>

<Navbar />

{#each allData as data}
    <div class="relative inline-block m-3 my-1 bg-white w-[20rem] left-5 sm:w-96 h-32 drop-shadow-md top-40 md:left-3 sm:left-28">
        <h2 class="relative text-xl font-extrabold p-3 left-3">{data.name}</h2>
        <p class="relative left-6 -top-2">{data.description.length > 20 ? data.description.substring(0,20) + "..." : data.description}</p>
        <p class="relative inline-block text-sm left-3 mx-3 opacity-60 left-1 top-5"><i class="fa-solid fa-clock"></i> {data.date}
        <p class="relative inline-block text-sm left-3 opacity-60 left-1 top-5"><i class="fa-solid fa-hashtag"></i>{data.id}</p>
        {#if user}
            <a class="relative float-right -left-5 top-3 inline-block px-4 py-1 text-sm font-medium text-white bg-gray-700 rounded" href="/posts/{data.id}">
                <i class="fa-solid fa-eye"></i> View
            </a>
        {/if}
    </div>
{/each}