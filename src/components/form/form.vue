<template>
    <form>
        <slot></slot>
    </form>
</template>
<script>
    /* eslint-disable */
    import objectAssign from '../../utils/merge';
    export default {
        name: 'wyForm',
        props: {
            model: {
                type: Object
            },
            rules: {
                type: Object
            },
        },
        provide() {
            return {
                form : this
            };
        },
        created () {
            this.$on('on-form-item-add', (field) => {
                if (field) this.fields.push(field);
            });
            this.$on('on-form-item-remove', (field) => {
                if (field.prop) this.fields.splice(this.fields.indexOf(field), 1);
            });
        },
        mounted() {
            // this.$emit('click')
        },
        data () {
            return {
                fields: []
            };
        },
        methods: {
            // 公开方法：全部重置数据
            resetFields() {
                this.fields.forEach(field => {
                    field.resetField();
                });
            },
            // 公开方法：全部校验数据，支持 Promise
            validate(callback) {
                let promise;
                // if no callback, return promise
                if (typeof callback !== 'function' && window.Promise) {
                    promise = new window.Promise((resolve, reject) => {
                        callback = function(valid) {
                        valid ? resolve(valid) : reject(valid);
                        };
                    });
                }
                let valid = true;
                let count = 0;
                // 如果需要验证的fields为空，调用验证时立刻返回callback
                if (this.fields.length === 0 && callback) {
                    callback(true);
                }
                let invalidFields = {};
                this.fields.forEach(field => {
                    field.validate('', (message, field) => {
                        if (message) {
                            valid = false;
                        }
                        invalidFields = objectAssign({}, invalidFields, field);
                        if (typeof callback === 'function' && ++count === this.fields.length) {
                            callback(valid, invalidFields);
                        }
                    });
                });
                if (promise) {
                    return promise;
                }
            }
        },
    }
</script>
