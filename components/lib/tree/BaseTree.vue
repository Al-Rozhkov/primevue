<script>
import BaseComponent from 'primevue/basecomponent';
import { useStyle } from 'primevue/usestyle';

const styles = `
.p-tree-container {
    margin: 0;
    padding: 0;
    list-style-type: none;
    overflow: auto;
}

.p-treenode-children {
    margin: 0;
    padding: 0;
    list-style-type: none;
}

.p-tree-wrapper {
    overflow: auto;
}

.p-treenode-selectable {
    cursor: pointer;
    user-select: none;
}

.p-tree-toggler {
    cursor: pointer;
    user-select: none;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    overflow: hidden;
    position: relative;
    flex-shrink: 0;
}

.p-treenode-leaf > .p-treenode-content .p-tree-toggler {
    visibility: hidden;
}

.p-treenode-content {
    display: flex;
    align-items: center;
}

.p-tree-filter {
    width: 100%;
}

.p-tree-filter-container {
    position: relative;
    display: block;
    width: 100%;
}

.p-tree-filter-icon {
    position: absolute;
    top: 50%;
    margin-top: -0.5rem;
}

.p-tree-loading {
    position: relative;
    min-height: 4rem;
}

.p-tree .p-tree-loading-overlay {
    position: absolute;
    z-index: 1;
    display: flex;
    align-items: center;
    justify-content: center;
}

.p-tree-flex-scrollable {
    display: flex;
    flex: 1;
    height: 100%;
    flex-direction: column;
}

.p-tree-flex-scrollable .p-tree-wrapper {
    flex: 1;
}
`;

const classes = {
    root: ({ props }) => [
        'p-tree p-component',
        {
            'p-tree-selectable': props.selectionMode != null,
            'p-tree-loading': props.loading,
            'p-tree-flex-scrollable': props.scrollHeight === 'flex'
        }
    ],
    loadingOverlay: 'p-tree-loading-overlay p-component-overlay',
    loadingIcon: 'p-tree-loading-icon',
    filterContainer: 'p-tree-filter-container',
    input: 'p-tree-filter p-inputtext p-component',
    searchIcon: 'p-tree-filter-icon',
    wrapper: 'p-tree-wrapper',
    container: 'p-tree-container',
    node: ({ treenode }) => ['p-treenode', { 'p-treenode-leaf': treenode.leaf }],
    content: ({ treenode }) => [
        'p-treenode-content',
        treenode.node.styleClass,
        {
            'p-treenode-selectable': treenode.selectable,
            'p-highlight': treenode.checkboxMode ? treenode.checked : treenode.selected
        }
    ],
    toggler: 'p-tree-toggler p-link',
    togglerIcon: 'p-tree-toggler-icon',
    checkboxContainer: 'p-checkbox p-component',
    checkbox: ({ treenode }) => [
        'p-checkbox-box',
        {
            'p-highlight': treenode.checked,
            'p-indeterminate': treenode.partialChecked
        }
    ],
    checkboxIcon: 'p-checkbox-icon',
    nodeIcon: ({ treenode }) => ['p-treenode-icon', treenode.node.icon],
    label: 'p-treenode-label',
    subgroup: 'p-treenode-children'
};

const { load: loadStyle, unload: unloadStyle } = useStyle(styles, { id: 'primevue_tree_style', manual: true });

export default {
    name: 'BaseTree',
    extends: BaseComponent,
    props: {
        value: {
            type: null,
            default: null
        },
        expandedKeys: {
            type: null,
            default: null
        },
        selectionKeys: {
            type: null,
            default: null
        },
        selectionMode: {
            type: String,
            default: null
        },
        metaKeySelection: {
            type: Boolean,
            default: true
        },
        loading: {
            type: Boolean,
            default: false
        },
        loadingIcon: {
            type: String,
            default: undefined
        },
        filter: {
            type: Boolean,
            default: false
        },
        filterBy: {
            type: String,
            default: 'label'
        },
        filterMode: {
            type: String,
            default: 'lenient'
        },
        filterPlaceholder: {
            type: String,
            default: null
        },
        filterLocale: {
            type: String,
            default: undefined
        },
        scrollHeight: {
            type: String,
            default: null
        },
        level: {
            type: Number,
            default: 0
        },
        'aria-labelledby': {
            type: String,
            default: null
        },
        'aria-label': {
            type: String,
            default: null
        }
    },
    css: {
        classes
    },
    watch: {
        isUnstyled: {
            immediate: true,
            handler(newValue) {
                !newValue && loadStyle();
            }
        }
    }
};
</script>
