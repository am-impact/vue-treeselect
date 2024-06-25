<template>
    <div
        v-if="node.isBranch"
        class="vue-treeselect__option-arrow-container"
        @click="handleMouseDownOnArrow"
    >
        <Transition
            name="vue-treeselect__option-arrow--prepare"
            appear
        >
            <arrow-icon :class="arrowClass" />
        </Transition>
    </div>
    <div
        v-else-if="instance.hasBranchNodes"
        class="vue-treeselect__option-arrow-placeholder"
    >
        &nbsp;
    </div>
</template>

<script>
import ArrowIcon from './../icons/Arrow';

export default {
    name: 'vue-treeselect--arrow',
    inject: ['instance'],

    /**
     * Components
     */
    components: {
        ArrowIcon,
    },

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
        arrowClass() {
            return {
                'vue-treeselect__option-arrow': true,
                'vue-treeselect__option-arrow--rotated': this.shouldExpand,
            };
        },

        shouldExpand() {
            return this.node.isBranch && this.instance.shouldExpand(this.node);
        },
    },

    /**
     * Methods
     */
    methods: {
        /**
         * Handle mouse down on arrow
         */
        handleMouseDownOnArrow() {
            console.log('handleMouseDownOnArrow');
            this.instance.toggleExpanded(this.node);
        },
    },
};
</script>
