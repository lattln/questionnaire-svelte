<script>
    import InputField from "./InputField.svelte";
    import SelectField from "./SelectField.svelte";
    import MultipleChoiceField from "./MultipleChoiceField.svelte";

    export let question = {};
    export let onAnswerChange = (id, value) => {};
</script>

{#if question.type === "text"}
    <InputField
        id={question.id}
        label={question.label}
        value={question.value}
        placeholder={question.placeholder || ""}
        onInput={(value) => onAnswerChange(question.id, value)}
    />
{:else if question.type === "select"}
    <SelectField
        id={question.id}
        label={question.label}
        options={question.options || []}
        value={question.value}
        onChange={(value) => onAnswerChange(question.id, value)}
    />
{:else if question.type === "multiple-choice"}
    <MultipleChoiceField
        id={question.id}
        label={question.label}
        options={question.options || []}
        selectedValues={question.value || []}
        onChange={(values) => onAnswerChange(question.id, values)}
    />
{/if}
