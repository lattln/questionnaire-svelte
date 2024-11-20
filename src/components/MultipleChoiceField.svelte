<script>
    export let id = "";
    export let label = "";
    export let options = [];
    export let selectedValues = [];
    export let onChange = (values) => {};
    export let onLabelChange = (newLabel) => {};
    export let onOptionsChange = (newOptions) => {};

    function toggleOption(option) {
        const newSelectedValues = selectedValues.includes(option)
            ? selectedValues.filter((v) => v !== option)
            : [...selectedValues, option];
        onChange(newSelectedValues);
    }

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

    <div>
        {#each options as option, index}
            <div class="flex items-center gap-2">
                <input
                    id={`${id}-${option}`}
                    type="checkbox"
                    checked={selectedValues.includes(option)}
                    on:change={() => toggleOption(option)}
                    class="rounded border-gray-300 focus:ring-indigo-500"
                />
                <label for={`${id}-${option}`}>{option}</label>

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

    <button
        type="button"
        on:click={addOption}
        class="bg-blue-500 text-white px-4 py-2 rounded mt-2">Add Option</button
    >
</div>
