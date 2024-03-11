<template>
    <div class="min-h-screen flex items-center justify-center bg-gray-50 py-12 px-4 sm:px-6 lg:px-8">
        <div class="max-w-md w-full space-y-8">
            <div class="text-left">
                <button @click="goBack"
                    class="text-sm text-indigo-600 hover:text-indigo-500 focus:outline-none focus:underline">
                    Home
                </button>
            </div>

            <div>
                <h1 class="text-center text-3xl font-extrabold text-gray-900">Sign Up</h1>
            </div>
            <form @submit.prevent="signup" class="mt-8 space-y-6">
                <div class="rounded-md shadow-sm -space-y-px">
                    <div>
                        <label for="name" class="sr-only">Name</label>
                        <input id="name" v-model="name" name="name" type="text" autocomplete="name" required
                            class="appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-t-md focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
                            placeholder="Name">
                    </div>
                    <div>
                        <label for="email" class="sr-only">Email</label>
                        <input id="email" v-model="email" name="email" type="email" autocomplete="email" required
                            class="appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
                            placeholder="Email">
                    </div>
                    <div>
                        <label for="password" class="sr-only">Password</label>
                        <input id="password" v-model="password" name="password" type="password"
                            autocomplete="new-password" required
                            class="appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-b-md focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
                            placeholder="Password">
                    </div>
                </div>

                <div>
                    <button type="submit"
                        class="group relative w-full flex justify-center py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
                        Sign Up
                    </button>
                </div>
            </form>

            <div class="text-center">
                <button @click="goToLogin"
                    class="text-sm text-indigo-600 hover:text-indigo-500 focus:outline-none focus:underline">
                    Already have an account? Login
                </button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'SignUp',
    data() {
        return {
            name: '',
            email: '',
            password: ''
        };
    },
    methods: {
        async signup() {
            try {
                const response = await fetch('http://localhost:5000/signup', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify({
                        email: this.email,
                        password: this.password
                    })
                });
                if (!response.ok) {
                    throw new Error('Failed to login');
                }
                const data = await response.json();

                localStorage.setItem('nuxtToken', data.access_token);

                this.$router.push('/');
            } catch (error) {
                console.error('Error logging in:', error);

            }
        },
        goToLogin() {
            this.$router.push({ path: '/auth/login' });
        },
        goBack() {
            this.$router.push({ path: '/' });
        }
    }
};
</script>

<style></style>