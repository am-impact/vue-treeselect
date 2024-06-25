<template>
    <span :class="optionLabelProps?.labelClassName">
        {{ optionLabelProps?.node?.label }}
        <span
            v-if="shouldShowCount"
            :class="optionLabelProps?.countClassName"
        >
            ({count})
        </span>
    </span>
</template>

<script>
export default {
    name: 'vue-treeselect--label',
    inject: ['instance'],

    /**
     * Props
     */
    props: {
        optionLabelProps: {
            type: Object,
            required: true,
        },
    },

    /**
     * Computed
     */
    computed: {
        /**
         * Count
         */
        count() {
            return this.optionLabelProps?.count;
        },

        /**
         * Should show count
         */
        shouldShowCount() {
            return this.optionLabelProps?.shouldShowCount;
        },
    },
};
</script>
