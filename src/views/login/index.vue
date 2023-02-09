<script setup lang="ts">
import Motion from "./utils/motion";
import { useRouter } from "vue-router";
import { message } from "@/utils/message";
import { loginRules } from "./utils/rule";
import { initRouter } from "@/router/utils";
import type { FormInstance } from "element-plus";
import { useLayout } from "@/layout/hooks/useLayout";
import { useUserStoreHook } from "@/store/modules/user";
import { bg } from "./utils/static";
import { useRenderIcon } from "@/components/ReIcon/src/hooks";
import { ref, reactive, onMounted, onBeforeUnmount } from "vue";
import Lock from "@iconify-icons/ri/lock-fill";
import User from "@iconify-icons/ri/user-3-fill";
import { useDataThemeChange } from "@/layout/hooks/useDataThemeChange";
const { setLayoutThemeColor } = useDataThemeChange();

defineOptions({
  name: "Login"
});
const router = useRouter();
const loading = ref(false);
const ruleFormRef = ref<FormInstance>();

const { initStorage } = useLayout();
initStorage();

const ruleForm = reactive({
  username: "admin",
  password: "admin123"
});

const onLogin = async (formEl: FormInstance | undefined) => {
  loading.value = true;
  if (!formEl) return;
  await formEl.validate((valid, fields) => {
    if (valid) {
      useUserStoreHook()
        .loginByUsername({ username: ruleForm.username, password: "admin123" })
        .then(res => {
          if (res.success) {
            // 获取后端路由
            initRouter().then(() => {
              router.push("/");
              message("登录成功", { type: "success" });
            });
          }
        });
    } else {
      loading.value = false;
      return fields;
    }
  });
};

/** 使用公共函数，避免`removeEventListener`失效 */
function onkeypress({ code }: KeyboardEvent) {
  if (code === "Enter") {
    onLogin(ruleFormRef.value);
  }
}

onMounted(() => {
  window.document.addEventListener("keypress", onkeypress);
  setLayoutThemeColor();
});

onBeforeUnmount(() => {
  window.document.removeEventListener("keypress", onkeypress);
});
</script>

<template>
  <div class="select-none h-screen w-screen">
    <img :src="bg" class="h-screen w-screen fixed -z-10" />
    <div class="flex-c h-full">
      <div class="login-description flex-dc flex-1 max-md:hidden">
        <div class="w-2/3">
          <div class="title">
            <Motion>
              <h1 class="text-6xl font-bold text-white">后台管理系统模板</h1>
            </Motion>
          </div>
          <Motion :delay="250">
            <div
              class="text-base text-gray-200 font-light mt-8 indent-8 leading-7"
            >
              这是简介这是简介这是简介这是简介这是简介这是简介这是简介这是简介这是简介这是简介
              这是简介这是简介这是简介这是简介这是简介这是简介这是简介
              这是简介这是简介这是简介这是简介这是简介这是简介这是简介
              这是简介这是简介这是简介这是简介
            </div>
          </Motion>
        </div>
      </div>
      <div class="flex-c flex-1 h-full">
        <el-card
          class="login-form flex-dc 2xl:w-2/3 xl:w-1/2 max-md:w-11/12 md:max-w-md min-h-1/2 px-1 py-11 !rounded-xl shadow-2xl"
        >
          <Motion>
            <h2 class="outline-none mb-8">登录</h2>
          </Motion>

          <el-form
            ref="ruleFormRef"
            :model="ruleForm"
            :rules="loginRules"
            class="w-full"
            size="large"
          >
            <Motion :delay="100">
              <el-form-item
                class="!mb-10"
                :rules="[
                  {
                    required: true,
                    message: '请输入账号',
                    trigger: 'blur'
                  }
                ]"
                prop="username"
              >
                <el-input
                  v-model="ruleForm.username"
                  placeholder="账号"
                  :prefix-icon="useRenderIcon(User)"
                />
              </el-form-item>
            </Motion>

            <Motion :delay="150">
              <el-form-item prop="password" class="!mb-10">
                <el-input
                  show-password
                  v-model="ruleForm.password"
                  placeholder="密码"
                  :prefix-icon="useRenderIcon(Lock)"
                />
              </el-form-item>
            </Motion>

            <Motion :delay="250">
              <el-button
                class="w-full !h-12 shadow-xl"
                size="default"
                type="primary"
                :loading="loading"
                @click="onLogin(ruleFormRef)"
              >
                登录
              </el-button>
            </Motion>
          </el-form>
        </el-card>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
:deep(.el-input-group__append, .el-input-group__prepend) {
  padding: 0;
}
</style>
