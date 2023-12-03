<script lang="ts" setup>
import { useAuthStore } from "~/stores/auth"
const authStore = useAuthStore();
const isAuthenticated = ref();
const router = useRouter();
isAuthenticated.value = useCookie("access_token").value;
const logout = async () => {
await authStore.logout();
const accessToken = useCookie("access_token");
const refreshToken = useCookie("refresh_token");
accessToken.value = null;
refreshToken.value = null;
setTimeout(() => {
isAuthenticated.value = useCookie("access_token").value;
}, 100);
router.push({
path: "/"
})
}
</script>

<template>
    <header class="w-full border-b border-slate-200 py-6 bg-gray-700">
        <div class="container">
            <div class="flex justify-between items-center">
                <div class="max-w-sm rounded ">
                <img class="w-28 h-18" src="gaming.png">
                </div>
                <nav class="flex items-center gap-6 text-white">
                    <NuxtLink to="/" class="text-base">Home</NuxtLink>
                    <p>|</p>
                    <NuxtLink to="/product" class="text-base">Products</NuxtLink>
                    <p>|</p>
                    <NuxtLink to="/cart" class="text-base">Cart</NuxtLink>
                    <p>|</p>
                    <NuxtLink v-if="!isAuthenticated" to="/login" class="text-base bg-blue-600 px-6 py-2 text-white rounded-lg hover:bg-blue-600/80">Login</NuxtLink>
                    <div v-else class="text-base cursor-pointer bg-red-600 px-6 py-2 text-white rounded-lg hover:bg-red-600/80" @click="logout">Logout</div>
                </nav>
            </div>
        </div>
    </header>
</template>