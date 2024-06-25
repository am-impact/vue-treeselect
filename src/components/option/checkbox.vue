<template>
    <span
        :aria-checked="ariaChecked"
        :class="checkboxClass"
        role="checkbox"
    >
        <span class="vue-treeselect__check-mark" />
        <span class="vue-treeselect__minus-mark" />
    </span>
</template>

<script>
import { UNCHECKED, INDETERMINATE, CHECKED } from './../../constants.js';

export default {
    name: 'vue-treeselect--checkbox',
    inject: ['instance'],

    /**
     * Props
     */
    props: {
        // The node object
        node: {
            type: Object,
            required: true,
        },
    },

    /**
     * Computed
     */
    computed: {
        /**
         * Aria checked attribute
         */
        ariaChecked() {
            return this.checkedState === CHECKED ? 'true' : 'false';
        },

        /**
         * Css classes
         */
        checkboxClass() {
            return {
                'vue-treeselect__checkbox': true,
                'vue-treeselect__checkbox--checked': this.checkedState === CHECKED,
                'vue-treeselect__checkbox--indeterminate': this.checkedState === INDETERMINATE,
                'vue-treeselect__checkbox--unchecked': this.checkedState === UNCHECKED,
                'vue-treeselect__checkbox--disabled': this.node.isDisabled,
            };
        },

        /**
         * Checked state
         */
        checkedState() {
            return this.instance.forest.checkedStateMap[this.node.id];
        },
    },

    /**
     * Methods
     */
    methods: {
        /**
         * Mouseenter event
         * @param {event} evt
         */
        handleMouseEnterOption(evt) {
            const { instance, node } = this;

            // Equivalent to `self` modifier.
            // istanbul ignore next
            if (evt.target !== evt.currentTarget) return;

            instance.setCurrentHighlightedOption(node, false);
        },
    },
};
</script>
