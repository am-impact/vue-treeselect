<template>
    <div>
        <template v-if="node.childrenStates.isLoaded">
            <treeselect-option
                v-for="childNode in node.children"
                :key="childNode.id"
                :node="childNode"
            />
        </template>
        <treeselect-tip
            v-if="showNoChildrenTip"
            type="no-children"
            icon="warning"
        >
            {{ instance.noChildrenText }}
        </treeselect-tip>
        <treeselect-tip
            v-if="showLoadingChildrenTip"
            type="loading"
            icon="loader"
        >
            {{ instance.loadingText }}
        </treeselect-tip>

        <treeselect-tip
            v-if="showErrorLoadingChildrenTip"
            type="error"
            icon="error"
        >
            {{ node.childrenStates.loadingError }}
            <a
                class="vue-treeselect__retry"
                :title="instance.retryTitle"
                @mousedown="handleMouseDownOnRetry"
            >
                {{ instance.retryText }}
            </a>
        </treeselect-tip>
    </div>
</template>

<script>
import TreeselectOption from './index.vue';
import TreeselectTip from './../Tip.vue';
import { onLeftClick } from './../../utils/index.js';

export default {
    name: 'vue-treeselect--sub',
    inject: ['instance'],

    /**
     * Components
     */
    components: {
        TreeselectOption,
        TreeselectTip,
    },

    /**
     * Props
     */
    props: {
        // Node
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
         * Error tonen
         */
        showErrorLoadingChildrenTip() {
            return this.node.childrenStates.loadingError;
        },

        /**
         * Loading tonen
         */
        showLoadingChildrenTip() {
            return this.node.childrenStates.isLoading;
        },

        /**
         * Geen children melding tonen
         */
        showNoChildrenTip() {
            if (!this.node.childrenStates.isLoaded || this.node.children.length) {
                return false;
            }

            return true;
        },
    },

    /**
     * Methods
     */
    methods: {
        /**
         * Mouse down on retry
         */
        handleMouseDownOnRetry: onLeftClick(function handleMouseDownOnRetry() {
            const { instance, node } = this;

            instance.loadChildrenOptions(node);
        }),
    },
};
</script>
