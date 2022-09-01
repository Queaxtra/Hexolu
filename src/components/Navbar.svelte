<script>
    import { createClient } from '@supabase/supabase-js';
    const supabase = createClient(import.meta.env.VITE_SUPABASE_URL, import.meta.env.VITE_SUPABASE_KEY);
    const user = supabase.auth.user()

    async function getDiscordLogin() {
        await supabase.auth.signIn({
            provider: 'discord',
        })
    }

    async function getLogOut() {
        await supabase.auth.signOut()
        document.location.reload(true);
    }
</script>

<div class="w-auto h-16 bg-white drop-shadow-md">
    <ul class="text-center block relative top-4 invisible md:visible sm:visible">
        <li class="relative inline m-3">
            <a href="/" class="opacity-70 hover:opacity-100"><i class="fa-solid fa-house relative"></i> Home</a>
        </li>
        {#if user}
            <li class="relative inline m-3">
                <a href="/share" class="opacity-70 hover:opacity-100"><i class="fa-solid fa-share"></i> Share</a>
            </li>
        {/if}
    </ul>
    <div class="float-right">
        <div class="dropdown dropdown-end">
            <button tabindex="0" class="inline-block px-4 py-3 text-sm font-medium text-black bg-gray-300 rounded relative -top-3.5 -left-2 visible sm:invisible md:invisible">
                <i class="fa-solid fa-bars"></i>
            </button>
            <ul tabindex="0" class="relative dropdown-content menu p-2 drop-shadow-md bg-white rounded w-52">
                <li>
                    <a href="/" class="opacity-70 hover:opacity-100"><i class="fa-solid fa-house relative"></i> Home</a>
                </li>
                {#if user}
                    <li>
                        <a href="/share" class="opacity-70 hover:opacity-100"><i class="fa-solid fa-share"></i> Share</a>
                    </li>
                {/if}
            </ul>
        </div>
        {#if user}
            <button on:click={getLogOut} class="inline-block px-4 py-3 text-sm font-medium text-white bg-red-500 rounded relative -top-3.5 -left-2">
                <i class="fa-solid fa-right-from-bracket"></i>
            </button>
        {:else}
            <button on:click={getDiscordLogin} class="inline-block px-4 py-3 text-sm font-medium text-white bg-green-500 rounded relative -top-3.5 -left-2">
                <i class="fa-solid fa-right-to-bracket"></i>
            </button>
        {/if}
    </div>
</div>