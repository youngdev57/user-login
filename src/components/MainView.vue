<template>
<section>
    <h2>메인 페이지</h2>
    <p>메인 페이지입니다.</p>
    <button @click="logout">로그아웃</button>
</section>
</template>

<script>
export default {
    name: "MainView",
    
    methods: {
        logout() {
            localStorage.removeItem("option-remember");
            localStorage.removeItem("user-password");
            this.$emit("logout");
        }
    }
}
</script>

<style scoped>
</style>