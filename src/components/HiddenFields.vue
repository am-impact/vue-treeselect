<template>
    <div v-if="instance.name && !instance.disabled && instance.hasValue">
        <template
            v-for="(stringifiedValue, i) in processedValues"
            :key="'hidden-field-' + i"
        >
            <input
                type="hidden"
                :name="instance.name"
                :value="stringifiedValue"
            />
        </template>
    </div>
</template>

<script>
import { isNaN } from '../utils/index.js';
import { defineComponent } from 'vue';

function stringifyValue(value) {
    if (typeof value === 'string') return value;
    if (value != null && !isNaN(value)) return JSON.stringify(value);
    return '';
}

export default defineComponent({
    name: 'vue-treeselect--hidden-fields',
    inject: ['instance'],
    computed: {
        processedValues() {
            let stringifiedValues = this.instance.internalValue.map(stringifyValue);
            if (this.instance.multiple && this.instance.joinValues) {
                stringifiedValues = [stringifiedValues.join(this.instance.delimiter)];
            }
            return stringifiedValues;
        },
    },
});
</script>
