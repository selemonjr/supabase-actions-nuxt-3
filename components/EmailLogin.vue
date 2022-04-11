<script setup lang="ts">
const client = useSupabaseClient();
const user = useSupabaseUser();
const loadingLogin = ref(false);
const loadingEmailLogin = ref(false);
const router = useRouter();
const email = ref("");
const password = ref("");
const errorMsg = ref("");
const handleEmailLogin = async () => {
    loadingEmailLogin.value = true;
        try {
            let { error } = await client.auth.signIn({
            email: email.value,
            password: password.value,
        });
        loadingEmailLogin.value = false;
        if(error) throw error;
        router.push("/")
        } catch (error) {
            errorMsg.value = `Error: ${error.message}`;
            setTimeout(() => {
                errorMsg.value = null;
            },2000)
        }
};
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
<div>
      <section  class="gridBox place-items-center w-full h-full">
        <div class="border border-t-gray-100 mt-20 mb-5 w-80 sm:w-full max-w-sm shadow-2xl rounded-md">
            <form class="bg-white w-full px-6 py-4 pb-5" id="form" @submit.prevent="handleEmailLogin">
                      <div class="text-center mt-2 mb-2">
                 <img class="w-full h-10 md:h-14" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/nuxtjs/nuxtjs-original.svg" />
            </div>
                <div class="pb-4 pt-2">
                    <label class="font-bold block pb-2">Email</label>
                    <input type="email" id="email" v-model="email" class="py-2 px-2 appearance-none focus:outline-none rounded-md w-full border border-gray-300 shadow-sm" placeholder="Email" required>
                </div>
                <div class="pb-3">
                    <label class="font-bold block pb-2">Password</label>
                    <input type="password" id="password" v-model="password" class="py-2 px-2 appearance-none focus:outline-none rounded-md w-full border border-gray-300 shadow-sm" placeholder="Password">
                </div>
                <div class="pb-1">
                    <p class="text-red-500 font-bold" id="error" v-if="errorMsg">{{errorMsg}}</p>
                </div>
                <div class="py-2">
                  <h3 class="font-bold text-md">Don't have an account yet? <router-link to="/register" class="text-green-500">Register</router-link></h3>
                </div>
                <div class="pb-4 pt-1">
                   <button type="submit" class="py-2 font-bold hover:bg-green-600 px-3 md:text-lg text-sm bg-green-500 text-white w-full shadow-md rounded-md">{{loadingEmailLogin ? "Signing In....." : "Sign In"}}</button>
                </div>
                    <div class="md:w-96 w-50 h-10 md:h-30 md:gridBox mb-5 md:mb-10">
                        <button @click="handleLogin" class="hover:border-green-600 bg-white hover:bg-gray-300 md:text-lg text-sm rounded-md border border-green-400 shadow-md py-2 md:py-2 px-1 md:px-2 w-64 md:w-80 ml-2 text-black font-bold">
                        <div class="flex justify-center items-center">
                            <div>
                                <img class="md:w-full w-10 h-10 pr-3" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/github/github-original.svg" />
                                </div>
                                <div>
                                    <h1 class="md:text-lg text-sm">{{loadingLogin ? "Loading...." : "Sign In With Github"}}</h1>
                                    </div>
                                    </div>
                                    </button>
                                    </div>
            </form>
        </div>
</section>
</div>
</template>