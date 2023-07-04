<script>
export default {
    name: 'vue-treeselect--placeholder',
    inject: ['instance'],

    render() {
        const { instance } = this;
        const placeholderClass = {
            'vue-treeselect__placeholder': true,
            'vue-treeselect-helper-zoom-effect-off': true,
            'vue-treeselect-helper-hide': instance.hasValue || instance.trigger.searchQuery,
        };

        return (
            <div class={placeholderClass} aria-label={instance.placeholder}>
                {instance.placeholder}
            </div>
        );
    },
};
</script>
