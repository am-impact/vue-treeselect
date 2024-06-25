<template>
    <div
        v-if="node.isBranch"
        class="vue-treeselect__option-arrow-container"
        @mouseover="handleMouseDownOnArrow"
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
import { onLeftClick } from './../../utils/index.js';
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
        node: {
            type: Object,
            required: true,
        },
    },

    /**
     * State
     */
    data() {
        return {};
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
     * Watch
     */
    watch: {
        // propName: function(newVal, oldVal) {},
    },

    /**
     * Created
     */
    created() {},

    /**
     * Mounted
     */
    mounted() {},

    /**
     * Methods
     */
    methods: {
        handleMouseDownOnArrow: onLeftClick(function handleMouseDownOnOptionArrow() {
            this.instance.toggleExpanded(this.node);
        }),
    },
};
</script>
