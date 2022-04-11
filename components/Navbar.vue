<script setup lang="ts">
const user = useSupabaseUser();
const client = useSupabaseClient();
const router = useRouter();
const handleLogOut = async () => {
    await client.auth.signOut();
    router.push("/login")
}
</script>
<template>
    <div class="w-full h-16 flex p-2 justify-between items-center bg-black border-b-2 border-gray-800">
        <div> 
            <img class="w-full h-10 md:h-14" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/nuxtjs/nuxtjs-original.svg" />
        </div>

        <div>
            <ul class="flex">
                <li class="padding"><nuxt-link to="/">{{user ? "Tasks" : "Home"}}</nuxt-link></li>
                <li class="padding" v-if="!user"><nuxt-link to="/login">Login</nuxt-link></li>
                  <li class="padding" v-if="user" @click="handleLogOut"><nuxt-link to="/login">LogOut</nuxt-link></li>
            </ul>
        </div>
    </div>
</template>

<style scoped>
.padding {
    @apply md:p-4 p-2 text-white md:font-bold text-sm md:text-lg font-roboto
}
</style>