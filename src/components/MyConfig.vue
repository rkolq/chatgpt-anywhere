<template>
    <el-form :model="form" :rules="rules" ref="ruleFormRef" label-width="100px">
        <el-form-item label="Key" prop="key">
            <el-input v-model.trim="form.key" type="password" />
        </el-form-item>
        <el-form-item label="请求域名" prop="baseUrl">
            <el-select v-model.trim="form.baseUrl" clearable allow-create filterable>
                <el-option label="https://api.openai.com" value="https://api.openai.com" />
                <el-option label="https://openai.api2d.net" value="https://openai.api2d.net" />
                <el-option label="https://stream.api2d.net" value="https://stream.api2d.net" />
            </el-select>
        </el-form-item>
        <el-form-item label="模型" prop="model">
            <el-select v-model.trim="form.model" clearable allow-create filterable>
                <el-option label="gpt-3.5-turbo" value="gpt-3.5-turbo" />
                <el-option label="gpt-3.5-turbo-0301" value="gpt-3.5-turbo-0301" />
                <el-option label="gpt-4" value="gpt-4" />
            </el-select>
        </el-form-item>
        <el-form-item label="默认引导" prop="systemRole">
            <el-input v-model.trim="form.systemRole" />
        </el-form-item>
        <el-form-item>
            <el-button type="primary" @click="submitForm(ruleFormRef)">保存</el-button>
        </el-form-item>
    </el-form>
</template>

<script setup lang="ts">
import { reactive, ref } from 'vue'
import type { FormInstance, FormRules } from 'element-plus'
import { klona } from 'klona/json';
import { useConfig } from '../hooks/useConfig';

const { getUnrefConf, setConfig } = useConfig();
const ruleFormRef = ref<FormInstance>()

const form = reactive(klona(getUnrefConf()));

const rules = reactive<FormRules>({
    key: [
        { required: true, message: '请输入key', trigger: 'blur' },
    ],
    baseUrl: [
        { required: true, message: '请输入baseUrl', trigger: 'blur' },
    ],
    model: [
        { required: true, message: '请输入model', trigger: 'blur' },
    ],
    systemRole: [
        { required: true, message: '请输入systemRole', trigger: 'blur' },
    ],
})

const submitForm = async (formEl: FormInstance | undefined) => {
    if (!formEl) return
    await formEl.validate((valid, fields) => {
        if (valid) {
            console.log(klona(form))
            setConfig(form)
        } else {
            console.log('error submit!', fields)
        }
    })
}
</script>