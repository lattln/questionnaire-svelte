<script>
    export let id = "";
    export let label = "";
    export let value = "";
    export let placeholder = "";
    export let onInput = (value) => {};
    export let onLabelChange = (newLabel) => {};
</script>

<div class="mb-4">
    <input
        type="text"
        bind:value
        {placeholder}
        on:input={(e) => onInput(e.currentTarget.value)}
        class="w-full border rounded px-3 py-2 focus:outline-none focus:ring focus:ring-indigo-500"
    />
    <input
        type="text"
        bind:value={label}
        on:input={(e) => onLabelChange(e.currentTarget.value)}
        class="block mt-2 w-full text-sm border p-2 rounded focus:outline-none focus:ring focus:ring-indigo-500"
        placeholder="Edit label"
    />
</div>
