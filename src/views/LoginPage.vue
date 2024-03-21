<template>
    <div class="login-container">
        <h1>Welcome to our Amazing Store!</h1>
        <button @click="handleClick" class="login-btn">Login</button>
    </div>
</template>

<script>    
export default {
    data() {
        return {
            isLoggedIn: false
        };
    },
    methods: {
        handleClick() {
            localStorage.setItem('isLoggedIn', 'true');
            this.isLoggedIn = true;
            this.$router.push({ name: 'shop' });
        }
    }
};
</script>



<style>
.login-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
    background-color: #f8f8f8;
}

h1 {
    font-size: 36px;
    margin-bottom: 20px;
    color: #333; 
}

.login-btn {
    padding: 15px 30px;
    font-size: 20px;
    background-color: #ff6f61; 
    color: #fff; 
    border: none;
    border-radius: 8px;
    cursor: pointer;
    transition: background-color 0.3s ease;
}

.login-btn:hover {
    background-color: #e94b3c; 
}
</style>
