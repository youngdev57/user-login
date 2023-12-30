<template>
<div class="app-container">
    <div class="component-wrapper">
        <component :is="currentComponent" @login="changeComponent('MainView')" @logout="changeComponent('UserLogin')" />
    </div>
</div>
</template>

<script>
import MainView from './components/MainView.vue'
import UserLogin from './components/user-login/UserLogin.vue'

export default {
    name: 'App',
        components: {
        MainView,
        UserLogin
    },
  
    data() {
        return {
            currentComponent: "UserLogin"
        }
    },

    methods: {
        changeComponent(component) {
            if (component)
                this.currentComponent = component;
        }
    }
}
</script>

<style>
.app-container {
    width: 100%;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #f6f6f6;
}
.component-wrapper {
    width: 400px;
    height: fit-content;
    padding: 20px;
    box-sizing: border-box;
    border-radius: 10px;
    background-color: #fff;
}
</style>
