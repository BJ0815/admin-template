<template>
  <div class="app-container">
    <el-form ref="form" :model="form" :rules="rules" label-width="120px">
      <el-form-item label="Title" prop="title">
        <el-input v-model="form.title" required />
      </el-form-item>
      <el-form-item label="Category">
        <el-select v-model="form.category" placeholder="please select your zone">
          <el-option label="News" value="news" />
          <el-option label="Products" value="products" />
        </el-select>
      </el-form-item>
      <el-form-item label="Activity time">
        <el-col :span="11">
          <el-date-picker v-model="form.date1" type="date" placeholder="Pick a date" style="width: 100%;" />
        </el-col>
        <el-col :span="2" class="line">-</el-col>
        <el-col :span="11">
          <el-time-picker v-model="form.date2" type="fixed-time" placeholder="Pick a time" style="width: 100%;" />
        </el-col>
      </el-form-item>
      <el-form-item prop="content" label="Content" style="margin-bottom: 30px;">
        <Tinymce ref="editor" v-model="form.content" :height="400" />
      </el-form-item>
      <el-form-item prop="image_uri" label="Photos" style="margin-bottom: 30px;">
        <Upload v-model="form.image_uri" />
      </el-form-item>
      <el-form-item>
        <el-button v-loading="loading" type="primary" @click="onSubmit">Create</el-button>
        <el-button v-loading="loading" @click="onCancel">Cancel</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
import Tinymce from '@/components/Tinymce'
import Upload from '@/components/Upload/SingleImage3'

export default {
  components: {
    Tinymce,
    Upload
  },
  data() {
    const validateRequire = (rule, value, callback) => {
      if (value === '') {
        this.$message({
          message: rule.field + '為必填',
          type: 'error'
        })
        callback(new Error(rule.field + '為必填'))
      } else {
        callback()
      }
    }

    return {
      form: {
        title: '',
        category: 'news',
        date1: '',
        date2: '',
        image_uri: '',
        content: ''
      },
      loading: false,
      rules: {
        image_uri: [{ validator: validateRequire }],
        title: [{ validator: validateRequire }],
        content: [{ validator: validateRequire }]
      }
    }
  },
  methods: {
    onSubmit() {
      this.$refs.form.validate(valid => {
        if (valid) {
          this.loading = true

          this.$notify({
            title: '成功',
            message: '建立文章完成',
            type: 'success',
            duration: 2000
          })
          this.loading = false
        } else {
          console.log('error submit!!')
          return false
        }
      })
    },
    onCancel() {
      this.$router.replace('/dashboard')
    }
  }
}
</script>

<style scoped>
.line{
  text-align: center;
}
</style>

