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
                <h1 class="text-center text-3xl font-extrabold text-gray-900">Login</h1>
            </div>
            <form @submit.prevent="login" class="mt-8 space-y-6">
          
                <div class="rounded-md shadow-sm -space-y-px">
   
                    <div>
                        <label for="email" class="sr-only">Email</label>
                        <input id="email" v-model="email" name="email" type="email" autocomplete="email" required
                            class="appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-t-md focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
                            placeholder="Email">
                    </div>
          
                    
                    <div>
                        <label for="password" class="sr-only">Password</label>
                        <input id="password" v-model="password" name="password" type="password"
                            autocomplete="current-password" required
                            class="appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-b-md focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 focus:z-10 sm:text-sm"
                            placeholder="Password">
                    </div>
                </div>

                <!-- Login Button -->
                <div>
                    <button type="submit"
                        class="group relative w-full flex justify-center py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
                        <span class="absolute left-0 inset-y-0 flex items-center pl-3">
                            <!-- Heroicon name: lock-closed -->
                            <svg class="h-5 w-5 text-indigo-500 group-hover:text-indigo-400"
                                xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor"
                                aria-hidden="true">
                                <path fill-rule="evenodd" d="M10 12a2 2 0 100-4 2 2 0 000 4z" clip-rule="evenodd" />
                                <path
                                    d="M4 8V6a4 4 0 118 0v2h1a3 3 0 013 3v5a2 2 0 01-2 2H6a2 2 0 01-2-2v-5a3 3 0 013-3h1z" />
                            </svg>
                        </span>
                        Login
                    </button>
                </div>
            </form>

            <!-- Signup Button -->
            <div class="text-center">
                <button @click="goToSignUp"
                    class="text-sm text-indigo-600 hover:text-indigo-500 focus:outline-none focus:underline">
                    Don't have an account? Sign Up
                </button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            email: '',
            password: ''
        };
    },
    methods: {
        async login() {
            try {
                const response = await fetch('http://localhost:5000/login', {
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
        goToSignUp() {
            this.$router.push({ path: '/auth/signup' });
        },
        goBack() {

            this.$router.push({ path: '/' });

        }
    }
};
</script>

<style>
/* Add your custom styles here */
</style>