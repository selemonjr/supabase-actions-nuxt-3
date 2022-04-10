<script setup lang="ts">
definePageMeta({
  layout: "custom",
});
const client = useSupabaseClient();
const user = useSupabaseUser();
const loadingLogin = ref(false);
const router = useRouter();
const handleLogin = async () => {
  loadingLogin.value = true;
  const { error } = await client.auth.signIn({provider: "github"})
  if(error) {
    return alert("Could Not Authenticate")
  } else {
        watchEffect(() => {
          if (user.value) {
          router.push('/')
  }
})
        loadingLogin.value = false;
  }
};
</script>
<template>
<div class="bg-black w-full min-h-screen gridBox">
    <div class="md:w-96 w-50 h-10 md:h-60 md:gridBox shadow-md rounded-md -mt-20 md:m-0">
        <button @click="handleLogin" class="bg-white hover:bg-gray-300 md:text-lg text-sm rounded-md shadow-md py-2 md:py-4 px-1 md:px-2 w-48 md:w-80 text-black font-bold">
          <div class="flex justify-center items-center">
            <div>
            <img class="md:w-full w-10 h-10 pr-3" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/github/github-original.svg" />
          </div>
          <div class="md:text-lg text-sm">
            {{loadingLogin ? "Loading...." : "Sign In With Github"}}
          </div>
          </div>
        </button>
    </div>
</div>
</template>