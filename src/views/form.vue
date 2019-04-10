<template>
    <div>
        <wy-form ref="form" :model="formValidate" :rules="ruleValidate">
            <wy-form-item label="用户名" prop="name">
                <wy-input v-model="formValidate.name"></wy-input>
            </wy-form-item>
            <wy-form-item label="邮箱" prop="mail">
                <wy-input v-model="formValidate.mail"></wy-input>
            </wy-form-item>
        </wy-form>
        <button @click="handleSubmit">提交</button>
        <button @click="handleReset">重置</button>
    </div>
</template>
<script>
    /* eslint-disable */
    import WyForm from '../components/form/form.vue';
    import WyFormItem from '../components/form/form-item.vue';
    import WyInput from '../components/input/input.vue';

    export default {
        name: 'form-page',
        components: { WyForm, WyFormItem, WyInput },
        data () {
            return {
                formValidate: {
                    name: '',
                    mail: ''
                },
                ruleValidate: {
                    name: [
                        { required: true, message: '用户名不能为空', trigger: 'blur' }
                    ],
                    mail: [
                        { required: true, message: '邮箱不能为空', trigger: 'blur' },
                        { type: 'email', message: '邮箱格式不正确', trigger: 'blur' }
                    ],
                }
            }
        },
        methods: {
            handleSubmit () {
                // this.$refs.form.validate((valid, invalidFields) => {
                //     if (valid) {
                //         window.alert('提交成功');
                //         console.log(invalidFields);
                //     } else {
                //         window.alert('表单校验失败');
                //         console.log(invalidFields);
                //     }
                // })
                this.$refs.form.validate()
                .then((res)=>{
                     window.alert('提交成功');
                })
                .catch((error) => {
                    window.alert('表单校验失败');
                })
            },
            handleReset () {
                this.$refs.form.resetFields();
            },
        }
    }
</script>
