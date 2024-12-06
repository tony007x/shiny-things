<script lang="ts">
    import Input from "$lib/components/ui/input/input.svelte";
    import wretch from "wretch";
    //@ts-ignore
    import { Mail, Lock, Github, Facebook, User } from "lucide-svelte";
    import { toast } from "svelte-sonner";
    import { goto } from "$app/navigation";
    let isRegister: boolean = false;

    let username: string;
    let email: string;
    let password: string;
    let confirmpassword: string;

    const login = async() => {
        const response = await wretch('api/v1/auth/login')
        .post({
            username: username,
            password: password
        })
        .badRequest(async(e)=>{
            toast.error("User not found!")
        })
        .json();

        if(response){
            goto("/home")
        }
    };

    const register = async () => {
        const response = await wretch("api/v1/auth/register")
        .post({
            username: username,
            email: email,
            password: password,
            confirmpassword: confirmpassword
        })
        .badRequest( async (e)=>{
            toast.error(JSON.parse(e.message).message)
        })
        .json();

        if(response){
            goto('/home')
        }
};
</script>

<div
    class={`flex w-full h-screen bg-[#FFF6E9] transform transition-transform duration-500 ${isRegister ? "flex-row-reverse" : ""}`}
>
    <!-- Left section with the login form -->
    <div class="flex w-2/3 justify-center items-center">
        <div
            class="flex flex-col w-full max-w-md mx-20 p-8 justify-around items-center h-screen"
        >
            <!-- Logo -->
            <img src="/logo.png" alt="Logo" class="w-70" />

            <!-- Form container -->
            <form class="flex flex-col w-full items-center gap-6">
                <h1 class="font-bold text-5xl text-center">
                    {isRegister ? "REGISTER" : "LOG IN"}
                </h1>

                <!-- Username Input with Icon -->
                <div class="relative w-full max-w-xs">
                    <Input
                        type="text"
                        placeholder="USERNAME"
                        class="pl-10 w-full shadow-md"
                        bind:value={username}
                        required
                    />
                    <User
                        class="absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-400"
                    />
                </div>

                {#if isRegister}
                    <!-- Extra Username Field for Register -->
                    <div class="relative w-full max-w-xs">
                        <Input
                            type="email"
                            placeholder="FULL NAME"
                            class="pl-10 w-full shadow-md"
                            required
                            bind:value={email}
                        />
                        <Mail
                            class="absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-400"
                        />
                    </div>
                {/if}

                <!-- Password Input with Icon -->
                <div class="relative w-full max-w-xs">
                    <Input
                        type="password"
                        placeholder="PASSWORD"
                        class="pl-10 w-full shadow-md"
                        required
                        bind:value={password}
                    />
                    <Lock
                        class="absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-400"
                    />
                </div>

                {#if isRegister}
                    <!-- Confirm Password Field -->
                    <div class="relative w-full max-w-xs">
                        <Input
                            type="password"
                            placeholder="CONFIRM PASSWORD"
                            class="pl-10 w-full shadow-md"
                            required
                            bind:value={confirmpassword}
                        />
                        <Lock
                            class="absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-400"
                        />
                    </div>
                {/if}

                <!-- Submit Button -->
                <button
                    on:click={() => (isRegister ? register() : login())}
                    type="submit"
                    class="p-2 w-1/2 rounded-2xl bg-[#40A2E3] text-white hover:bg-[#3280b4] duration-300 shadow-lg"
                >
                    {isRegister ? "REGISTER" : "LOGIN"}
                </button>
            </form>

            <!-- Line -->
            <div class="flex flex-col justify-center text-center w-full gap-3">
                <span class="border-b-2 border-black w-full mt-4"></span>
                <h1>or <br /> Login via Oauth</h1>
                <div class="flex w-full justify-around p-3">
                    <button
                        class="border rounded-full p-1 hover:bg-gray-500 duration-300"
                        ><img
                            src="/google.png"
                            alt="google"
                            class=" w-[35px] h-[25px]"
                        /></button
                    >
                    <button
                        class="border rounded-full p-2 hover:bg-gray-500 duration-300"
                        ><Facebook size="28px" /></button
                    >
                    <button
                        class="border rounded-full p-2 hover:bg-gray-500 duration-300"
                        ><Github size="28px" /></button
                    >
                </div>
            </div>
        </div>
    </div>

    <!-- Right section with a background color -->
    <div
        class="flex flex-col w-1/3 bg-[#40A2E3] relative justify-center items-center gap-10"
    >
        <img src="/triangle.png" alt="" class="absolute top-5 left-10" />
        <img
            src="/triangle.png"
            alt=""
            class="absolute right-14 top-[100px] rotate-45 w-16"
        />

        <img
            src="/triangle.png"
            alt=""
            class="absolute right-[100px] bottom-[20px] rotate-90 w-25"
        />

        <img src="/circle.png" alt="" class="absolute left-4 top-1/3" />
        <img
            src="/circle.png"
            alt=""
            class="absolute right-14 top-[600px] w-14"
        />

        <img
            src="/Rectangle.png"
            alt=""
            class="absolute right-0 top-[280px] w-20"
        />

        <img
            src="/Rectangle.png"
            alt=""
            class="absolute left-[50px] bottom-[120px] rotate-45 w-16 z-10"
        />

        <h1 class="text-5xl font-bold text-white text-center">
            {!isRegister ? "New Here?" : "Have an account?"}
        </h1>
        <span class="text-center text-white">
            {#if !isRegister}
                Enter your personal details <br /> and start journey with us.
            {:else}
                To keep connected with us please. <br />login with your personal
                info
            {/if}
        </span>

        <button
            class="p-2 w-[200px] rounded-2xl bg-[#D9D9D9] hover:bg-[#bebebe] shadow-md duration-300"
            on:click={() => {
                isRegister = !isRegister;
                username = "";
                email = "";
                password = "";
                confirmpassword = "";
            }}
        >
            {isRegister ? "LOGIN" : "REGISTER"}
        </button>
    </div>
</div>
