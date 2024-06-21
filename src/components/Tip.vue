<template>
    <div :class="wrapperClass">
        <div class="vue-treeselect__icon-container">
            <span :class="iconClass" />
        </div>
        <span :class="labelClass">
            <slot />
        </span>
    </div>
</template>

<script>
export default {
    name: 'vue-treeselect--tip',

    props: {
        type: {
            type: String,
            required: true,
        },
        icon: {
            type: String,
            required: true,
        },
    },

    computed: {
        iconClass() {
            return {
                [`vue-treeselect__icon-${this.icon}`]: true,
            };
        },

        labelClass() {
            return {
                'vue-treeselect__tip-text': true,
                [`vue-treeselect__${this.type}-tip-text`]: true,
            };
        },

        wrapperClass() {
            return {
                'vue-treeselect__tip': true,
                [`vue-treeselect__${this.type}-tip`]: true,
            };
        },
    },
};
</script>
