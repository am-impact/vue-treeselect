<template>
    <div
        role="option"
        :class="listItemClass"
    >
        <div
            :class="optionClass"
            :data-id="node.id"
            @mouseover="handleMouseEnterOption"
        >
            <option-arrow
                v-if="showArrow"
                :node="node"
            />

            <label
                class="vue-treeselect__label-container"
                @click="handleMouseDownOnLabelContainer"
            >
                <div
                    v-if="!instance.single || (instance.disableBranchNodes && node.isBranch)"
                    class="vue-treeselect__checkbox-container"
                >
                    <option-checkbox :node="node" />
                </div>

                <!--
                    TODO checken of slot werkt
                -->
                <slot
                    name="option-label"
                    v-bind="optionLabelProps"
                >
                    <option-label :option-label-props="optionLabelProps" />
                </slot>
            </label>
        </div>

        <Transition
            v-if="node.isBranch"
            name="vue-treeselect__list--transition"
        >
            <div
                v-if="shouldExpand"
                class="vue-treeselect__list"
            >
                <option-sub :node="node" />
            </div>
        </Transition>
    </div>
</template>

<script>
import OptionArrow from './arrow.vue';
import OptionCheckbox from './checkbox.vue';
import OptionLabel from './label.vue';
import OptionSub from './sub.vue';

export default {
    name: 'vue-treeselect--option',
    inject: ['instance'],

    /**
     * Components
     */
    components: {
        OptionArrow,
        OptionCheckbox,
        OptionLabel,
        OptionSub,
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
         * Css classes op list item

         */
        listItemClass() {
            const indentLevel = this.instance.shouldFlattenOptions ? 0 : this.node.level;

            return {
                'vue-treeselect__list-item': true,
                [`vue-treeselect__indent-level-${indentLevel}`]: true,
            };
        },

        /**
         * Css classes op option
         */
        optionClass() {
            return {
                'vue-treeselect__option': true,
                'vue-treeselect__option--disabled': this.node.isDisabled,
                'vue-treeselect__option--hidden': this.node.isHidden,
                'vue-treeselect__option--selected': this.instance.isSelected(this.node),
                'vue-treeselect__option--highlight': this.node.isHighlighted,
                'vue-treeselect__option--matched':
                    this.instance.localSearch.active && this.node.isMatched,
                'vue-treeselect__option--hide': !this.shouldShow,
            };
        },

        /**
         * Props voor label
         */
        optionLabelProps() {
            const shouldShowCount =
                this.node.isBranch &&
                (this.instance.localSearch.active
                    ? this.instance.showCountOnSearchComputed
                    : this.instance.showCount);
            const count = shouldShowCount
                ? this.instance.localSearch.active
                    ? this.instance.localSearch.countMap[this.node.id][this.instance.showCountOf]
                    : this.node.count[this.instance.showCountOf]
                : NaN;

            return {
                node: this.node,
                shouldShowCount: shouldShowCount,
                count: count,
                labelClassName: 'vue-treeselect__label',
                countClassName: 'vue-treeselect__count',
            };
        },

        /**
         * Meer opties?
         */
        shouldExpand() {
            const { instance, node } = this;

            return node.isBranch && instance.shouldExpand(node);
        },

        /**
         * Option tonen?
         */
        shouldShow() {
            const { instance, node } = this;

            return instance.shouldShowOptionInMenu(node);
        },

        /**
         * Uitklap arrow tonen?
         */
        showArrow() {
            const { instance } = this;

            return !(instance.shouldFlattenOptions && this.shouldShow);
        },
    },

    /**
     * Methods
     */
    methods: {
        /**
         * Mouse over option
         * @param {Event} evt
         */
        handleMouseEnterOption(evt) {
            const { instance, node } = this;

            // Equivalent to `self` modifier.
            // istanbul ignore next
            if (evt.target !== evt.currentTarget) return;

            instance.setCurrentHighlightedOption(node, false);
        },

        /**
         * Click op label container
         */
        handleMouseDownOnLabelContainer() {
            const { instance, node } = this;

            if (node.isBranch && instance.disableBranchNodes) {
                instance.toggleExpanded(node);
            } else {
                instance.select(node);
            }
        },
    },
};
</script>
