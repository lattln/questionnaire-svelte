<script>
    export let id = "";
    export let label = "";
    export let options = [];
    export let value = "";
    export let onChange = (value) => {};
    export let onLabelChange = (newLabel) => {};
    export let onOptionsChange = (newOptions) => {};

    function addOption() {
        options = [...options, ""];
        onOptionsChange(options);
    }

    function removeOption(index) {
        options.splice(index, 1);
        onOptionsChange(options);
    }

    function updateOption(index, newOption) {
        options[index] = newOption;
        onOptionsChange(options);
    }
</script>

<div class="mb-4">
    <input
        type="text"
        bind:value={label}
        on:input={(e) => onLabelChange(e.currentTarget.value)}
        class="block mb-2 w-full text-sm border p-2 rounded focus:outline-none focus:ring focus:ring-indigo-500"
        placeholder="Edit label"
    />

    <select
        {id}
        bind:value
        on:change={(e) => onChange(e.currentTarget.value)}
        class="w-full border rounded px-3 py-2 focus:outline-none focus:ring focus:ring-indigo-500"
    >
        <option value="" disabled>Select an option</option>
        {#each options as option, index}
            <option value={option}>{option}</option>
        {/each}
    </select>

    <div class="mt-2">
        <button
            type="button"
            on:click={addOption}
            class="bg-blue-500 text-white px-4 py-2 rounded">Add Option</button
        >
        {#each options as option, index}
            <div class="mt-1 flex items-center">
                <input
                    type="text"
                    value={option}
                    on:input={(e) => updateOption(index, e.currentTarget.value)}
                    class="block w-full text-sm border p-2 rounded focus:outline-none focus:ring focus:ring-indigo-500"
                    placeholder="Option"
                />
                <button
                    type="button"
                    on:click={() => removeOption(index)}
                    class="bg-red-500 text-white px-2 py-1 rounded ml-2"
                    >Remove</button
                >
            </div>
        {/each}
    </div>
</div>
