<template>
  <div class="container">
    <div class="title">
      Json 对象 → TypeScript Interface 在线转换器
    </div>
    <el-container style="display: block;width: 100%;">
      <el-main>
        <el-row class="">
          <el-col :span="16" :offset="4" class="elCol">
            <el-input
                v-model="jsonStr"
                :autosize="{ minRows: 14, maxRows: 20 }"
                type="textarea"
                placeholder="请输入json或者json数组"
            />
          </el-col>
          <el-col :span="20" :offset="2" class="elCol center">
            <el-button type="primary" @click="trans">点击转换</el-button>
          </el-col>
          <el-col :span="16" :offset="4" class="elCol">
            <el-input
                v-model="tsStr"
                :autosize="{ minRows: 14, maxRows: 20 }"
                type="textarea"
            />
          </el-col>
        </el-row>
      </el-main>
    </el-container>
  </div>
</template>

<script setup>
import {ref} from 'vue'
import {ElMessage} from "element-plus";
import JsonToTS from 'js-to-interface'

const jsonStr = ref('')
const tsStr = ref('')

const trans = () => {
  if (!jsonStr.value || jsonStr.value === '') {
    ElMessage.error('请输入正确的json或者json数组')
    return
  }
  tsStr.value = ''
  try {
    const data = JsonToTS(jsonStr.value, {fKey: 'json'})
    data.forEach((interFaceStr, idx) => {
      if (idx === 0) {
        tsStr.value += `${interFaceStr}`
      } else {
        tsStr.value += `\r\n${interFaceStr}`
      }
    });
  } catch (e) {
    ElMessage.error(e.message)
  }

}
</script>

<style scoped>
.container {
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.title {
  font-size: 30px;
  font-weight: 600;
  margin: 40px 0;
}

.center {
  display: flex;
  justify-content: center;
}

.elCol {
  margin-bottom: 30px;
}
</style>
