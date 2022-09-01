<script>
    import { createClient } from '@supabase/supabase-js';
    const supabase = createClient(import.meta.env.VITE_SUPABASE_URL, import.meta.env.VITE_SUPABASE_KEY);
    import Navbar from "../../components/Navbar.svelte";
    import toast, { Toaster } from 'svelte-french-toast';
    const time = new Date();
    const user = supabase.auth.user()

    let code = Math.floor(Math.random() * 1000000) + 100000;
    let newdate = time.getFullYear() + "-" + (time.getMonth() + 1) + "-" + time.getDate();
    let newblogname = "";
    let newblogdescription = "";
    let newblogcontents = "";

    async function shareBlog() {
        if(!newblogname && !newblogdescription && !newblogdescription) {
            toast.error('Please fill in all fields.', {
                style: 'background: #fafafa;',
                position: "bottom-right"
            })
        }

        await supabase.from('posts').insert([{ id: code, name: newblogname, description: newblogdescription, contents: newblogcontents, date: newdate }])
        toast.success("Successfully blog shared!", {
            style: 'background: #fafafa;',
            position: "bottom-right"
        })
    }

    async function getDiscordLogin() {
        await supabase.auth.signIn({
            provider: 'discord',
        })
    }
</script>

<Navbar />

{#if user}
    <Toaster />
    <div class="relative left-1/2 -translate-x-1/2 w-[22rem] sm:w-[33rem] h-96 bg-white drop-shadow-md top-44 md:top-40 sm:top-40">
        <div class="relative left-1/2 -translate-x-1/2 w-16 h-16 bg-blue-600/30 top-3 rounded-full">
            <i class="relative left-1/2 -translate-x-1/2 text-2xl top-4 text-blue-600 fa-solid fa-share"></i>
        </div>
        <h2 class="text-center text-2xl font-extrabold relative top-6">Share Post</h2>
        <div>
            <input type="text" bind:value={newblogname} class="w-72 md:w-[24rem] sm:w-[24rem] h-12 bg-gray-200/30 drop-shadow-md relative left-1/2 text-center -translate-x-1/2 top-10" placeholder="Title">
            <input type="text" bind:value={newblogdescription} class="w-72 md:w-[24rem] sm:w-[24rem] h-12 bg-gray-200/30 drop-shadow-md relative left-1/2 text-center -translate-x-1/2 top-12" placeholder="Description">
            <textarea bind:value={newblogcontents} class="w-72 md:w-[24rem] sm:w-[24rem] h-12 bg-gray-200/30 drop-shadow-md relative left-1/2 text-center -translate-x-1/2 top-14"></textarea>
            <div class="relative ml-[6rem] sm:ml-[11.7rem] md:ml-[11.7rem] top-20">
                <button on:click={shareBlog} class="inline-block px-8 py-3 text-sm font-medium text-white bg-green-500 rounded"><i class="fa-solid fa-share"></i></button>
                <button class="inline-block px-8 py-3 text-sm font-medium text-white bg-red-500 rounded"><i class="fa-solid fa-xmark"></i></button>
            </div>
        </div>
    </div>
{:else}
    <div class="relative left-1/2 -translate-x-1/2 w-60 sm:w-60 h-52 bg-white drop-shadow-md top-44 md:top-40 sm:top-40">
        <div class="relative left-1/2 -translate-x-1/2 w-16 h-16 bg-red-600/30 top-3 rounded-full">
            <i class="relative left-1/2 -translate-x-1/2 text-2xl top-4 text-red-600 fa-solid fa-xmark"></i>
        </div>
        <h2 class="text-center text-2xl font-extrabold relative top-6">Please Login!</h2>
        <button on:click={getDiscordLogin} class="relative left-1/2 -translate-x-1/2 inline-block px-4 py-3 text-sm font-medium text-white bg-green-500 rounded relative top-14">
            <i class="fa-solid fa-right-to-bracket"></i>
        </button>
    </div>
{/if}