<template>
<section class="login-container">
    <h2>사용자 로그인</h2>
    <Transition>
        <div v-show="message" class="message-wrapper">
            <img src="@/assets/caution.svg" width="18">
            <span>{{ message || "" }}</span>
        </div>
    </Transition>
    <div>
        <label for="email">이메일</label>
        <input type="text" v-model="user.email" ref="inpEmail" id="email" placeholder="email@example.com">
    </div>
    <div>
        <label for="password">비밀번호</label>
        <input type="password" v-model="user.password" ref="inpPassword" id="password">
    </div>
    <div class="option-wrapper">
        <input type="checkbox" v-model="option.remember" id="remember">
        <span @click="toggleOption" style="cursor: pointer">로그인 유지</span>
    </div>
    <div>
        <button @click="validate">로그인</button>
    </div>
</section>
</template>

<script>
import cryptoJs from "crypto-js"

export default {
    name: "UserLogin",

    data() {
        return {
            message: "",
            user: {
                email: "",
                password: ""
            },
            option: {
                remember: null
            },
            regExp: {
                checkEmail(email) {
                    return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
                },
                checkPassword(password) {
                    return /^(?=[a-zA-Z])(?=.*[a-z])(?=.*[A-Z])(?=.*\d)[a-zA-Z\d]+$/.test(password);
                }
            }
        }
    },

    created() {
        this.init();
    },

    methods: {
        init() {
            this.option.remember = localStorage.getItem("option-remember");
            this.user.email = localStorage.getItem("user-email");
            this.user.password = localStorage.getItem("user-password");

            if (this.option.remember)
                this.login();
        },

        validate() {
            if (!this.regExp.checkEmail(this.user.email)) {
                this.message = "이메일 형식에 맞지 않습니다.";
                return this.$refs.inpEmail.focus();
            }

            if (!this.regExp.checkPassword(this.user.password)) {
                this.message = "비밀번호를 다시 확인해주세요.";
                return this.$refs.inpPassword.focus();
            }

            this.option.remember
                ? localStorage.setItem("option-remember", this.option.remember)
                : localStorage.removeItem("option-remember");

            this.login();
        },

        login() {
            localStorage.setItem("user-email", this.user.email);
            localStorage.setItem("user-password", this.getHashed(this.user.password));

            this.$emit("login");
        },

        getHashed(data) {
            return cryptoJs.SHA256(data).toString(cryptoJs.enc.Hex);
        },

        toggleOption() {
            this.option.remember = !this.option.remember;
        }
    }
}
</script>

<style scoped>
@import "@/assets/styles/common.css";

.login-container {
    width: 100%;
    display: flex;
    flex-direction: column;
    gap: 15px;
}
.option-wrapper {
    height: 30px;
    font-size: 0.9rem;
    display: flex;
    align-items: center;
    gap: 3px;
}
.message-wrapper {
    padding: 10px;
    box-sizing: border-box;
    background: rgb(255, 237, 237);
    font-size: 0.8rem;
    border-radius: 5px;
    color: #e52a2a;
    display: flex;
    align-items: center;
    gap: 10px;
}
.v-enter-active,
.v-leave-active {
    transition: opacity 0.5s ease;
}
.v-enter-from,
.v-leave-to {
    opacity: 0;
}
</style>