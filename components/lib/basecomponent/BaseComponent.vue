<script>
import { ObjectUtils } from 'primevue/utils';
import { mergeProps } from 'vue';

const inlineStyles = {
    hiddenAccessible: {
        border: '0',
        clip: 'rect(0 0 0 0)',
        height: '1px',
        margin: '-1px',
        overflow: 'hidden',
        padding: '0',
        position: 'absolute',
        width: '1px'
    }
};

export default {
    name: 'BaseComponent',
    props: {
        pt: {
            type: Object,
            default: undefined
        },
        unstyled: {
            type: Boolean,
            default: undefined
        }
    },
    methods: {
        _getOptionValue(options, key = '', params = {}) {
            const fKeys = ObjectUtils.convertToFlatCase(key).split('.');
            const fKey = fKeys.shift();

            return fKey
                ? ObjectUtils.isObject(options)
                    ? this._getOptionValue(ObjectUtils.getItemValue(options[Object.keys(options).find((k) => ObjectUtils.convertToFlatCase(k) === fKey) || ''], params), fKeys.join('.'), params)
                    : undefined
                : ObjectUtils.getItemValue(options, params);
        },
        _getPTValue(obj = {}, key = '', params = {}) {
            const datasetPrefix = 'data-pc-';
            const self = this._getOptionValue(obj, key, params);
            const globalPT = this._getOptionValue(this.defaultPT, key, params);
            const merged = mergeProps(self, globalPT, {
                ...(key === 'root' && { [`${datasetPrefix}name`]: ObjectUtils.convertToFlatCase(this.$.type.name) }),
                [`${datasetPrefix}section`]: ObjectUtils.convertToFlatCase(key)
            });

            return merged;
            /*
             * @todo: The 'class' option in self can always be more powerful to style the component easily.
             *
             * return self && self['class'] ? { ...merged, ...{ class: self['class'] } } : merged;
             */
        },
        ptm(key = '', params = {}) {
            return this._getPTValue(this.pt, key, { props: this.$props, state: this.$data, ...params });
        },
        ptmo(obj = {}, key = '', params = {}) {
            return this._getPTValue(obj, key, params);
        },
        cx(key = '', params = {}) {
            return !this.isUnstyled ? this._getOptionValue(this.$options.css && this.$options.css.classes, key, { instance: this, props: this.$props, state: this.$data, ...params }) : undefined;
        },
        cxo(obj = {}, key = '', params = {}) {
            // @todo
            return !this.isUnstyled ? this._getOptionValue(obj.css && obj.css.classes, key, { instance: obj, props: obj && obj.props, state: obj && obj.data, ...params }) : undefined;
        },
        sx(key = '', when = true, params = {}) {
            if (when) {
                const self = this._getOptionValue(this.$options.css && this.$options.css.inlineStyles, key, { instance: this, props: this.$props, state: this.$data, ...params });
                const base = this._getOptionValue(inlineStyles, key, { instance: this, props: this.$props, state: this.$data, ...params });

                return [base, self];
            }

            return undefined;
        }
    },
    computed: {
        defaultPT() {
            return this._getOptionValue(this.$primevue.config.pt, this.$.type.name, this.defaultsParams);
        },
        defaultsParams() {
            return { instance: this };
        },
        isUnstyled() {
            return this.unstyled !== undefined ? this.unstyled : this.$primevue.config.unstyled;
        }
    }
};
</script>
