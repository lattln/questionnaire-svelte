<script>
    import QuestionField from "./QuestionField.svelte";
    import { writable } from "svelte/store";

    let questions = writable([
        {
            id: "name",
            type: "text",
            label: "Name",
            value: "",
            placeholder: "Enter your name",
        },
        {
            id: "email",
            type: "text",
            label: "Email",
            value: "",
            placeholder: "Enter your email",
        },
        {
            id: "favorite-color",
            type: "select",
            label: "Favorite Color",
            value: "",
            options: ["Red", "Blue", "Green"],
        },
        {
            id: "hobbies",
            type: "multiple-choice",
            label: "Hobbies",
            value: [],
            options: ["Reading", "Gaming", "Traveling"],
        },
    ]);

    let previewMode = false;

    function addField(type) {
        const newField =
            type === "text"
                ? {
                      id: `field-${Date.now()}`,
                      type: "text",
                      label: "New Text Field",
                      value: "",
                      placeholder: "Enter text",
                  }
                : type === "select"
                  ? {
                        id: `field-${Date.now()}`,
                        type: "select",
                        label: "New Select Field",
                        value: "",
                        options: ["Option 1", "Option 2"],
                    }
                  : {
                        id: `field-${Date.now()}`,
                        type: "multiple-choice",
                        label: "New Multiple Choice Field",
                        value: [],
                        options: ["Option 1", "Option 2"],
                    };

        questions.update((q) => [...q, newField]);
    }

    function removeField(id) {
        questions.update((q) => q.filter((question) => question.id !== id));
    }

    function togglePreview() {
        previewMode = !previewMode;
    }
</script>

<div class="space-y-4">
    <!-- Controls -->
    <div class="flex gap-4">
        <button
            on:click={() => addField("text")}
            class="bg-blue-500 text-white px-4 py-2 rounded"
        >
            Add Text Field
        </button>
        <button
            on:click={() => addField("select")}
            class="bg-green-500 text-white px-4 py-2 rounded"
        >
            Add Select Field
        </button>
        <button
            on:click={() => addField("multiple-choice")}
            class="bg-orange-500 text-white px-4 py-2 rounded"
        >
            Add Multiple Choice Field
        </button>
        <button
            on:click={togglePreview}
            class="bg-gray-500 text-white px-4 py-2 rounded"
        >
            {previewMode ? "Exit Preview" : "Preview"}
        </button>
    </div>

    {#if previewMode}
        <div class="space-y-4">
            {#each $questions as question (question.id)}
                <div>
                    <label for={question.id}>{question.label}</label>
                    {#if question.type === "text"}
                        <input
                            id={question.id}
                            type="text"
                            value={question.value}
                            placeholder={question.placeholder}
                            readonly
                            class="w-full border rounded px-3 py-2"
                        />
                    {:else if question.type === "select"}
                        <select
                            id={question.id}
                            value={question.value}
                            class="w-full border rounded px-3 py-2"
                            disabled
                        >
                            {#each question.options as option}
                                <option value={option}>{option}</option>
                            {/each}
                        </select>
                    {:else if question.type === "multiple-choice"}
                        <div>
                            {#each question.options as option}
                                <label>
                                    <input
                                        type="checkbox"
                                        checked={question.value.includes(
                                            option,
                                        )}
                                        disabled
                                    />
                                    {option}
                                </label>
                            {/each}
                        </div>
                    {/if}
                </div>
            {/each}
        </div>
    {:else}
        <form class="space-y-4">
            {#each $questions as question (question.id)}
                <QuestionField
                    {question}
                    onAnswerChange={(id, value) => {
                        questions.update((q) => {
                            const field = q.find((item) => item.id === id);
                            if (field) field.value = value;
                            return q;
                        });
                    }}
                />
                <button
                    on:click={() => removeField(question.id)}
                    type="button"
                    class="text-red-500 underline"
                >
                    Remove
                </button>
            {/each}
        </form>
    {/if}
</div>
