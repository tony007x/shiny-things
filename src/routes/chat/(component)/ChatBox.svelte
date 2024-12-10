<script>
    let messages = [
        { sender: "user", text: "พีคเป็นเกย์จริงมั้ย" },
        { sender: "receiver", text: "จริงๆ มันบอกเอง" },
    ];
</script>

<div class="flex flex-col h-full">
    <div class="flex-grow p-4 space-y-4 overflow-y-auto">
        {#each messages as message}
            <div
                class={`flex ${message.sender === "user" ? "justify-start" : "justify-end"}`}
            >
                <div
                    class={`p-2 rounded-lg max-w-xs ${message.sender === "user" ? "bg-blue-500 text-white" : "bg-gray-200 text-black"}`}
                >
                    <p>{message.text}</p>
                </div>
            </div>
        {/each}
    </div>
    <div class="p-4 flex items-center border-t border-gray-300">
        <input
            type="text"
            placeholder="Aa"
            class="w-full p-2 border border-gray-300 rounded-lg bg-white"
        />
        <button
            class="ml-2 p-2 bg-blue-500 text-white rounded-lg hover:bg-blue-600"
            >Send</button
        >
    </div>
</div>
