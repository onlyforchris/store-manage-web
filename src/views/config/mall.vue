<template>
  <div class="app-container">
    <el-form
      ref="dataForm"
      :rules="rules"
      :model="dataForm"
      status-icon
      label-width="300px"
    >
      <el-form-item
        :label="$t('config_mall.form.mall_name')"
        prop="waterstore_mall_name"
      >
        <el-input v-model="dataForm.waterstore_mall_name" />
      </el-form-item>
      <el-form-item
        :label="$t('config_mall.form.mall_address')"
        prop="waterstore_mall_address"
      >
        <el-input v-model="dataForm.waterstore_mall_address" />
      </el-form-item>
      <el-form-item :label="$t('config_mall.form.mall_coordinates')">
        <el-col :span="11">
          <el-input
            v-model="dataForm.waterstore_mall_longitude"
            :placeholder="$t('config_mall.placeholder.mall_longitude')"
          />
        </el-col>
        <el-col :span="2" style="text-align: center">-</el-col>
        <el-col :span="11">
          <el-input
            v-model="dataForm.waterstore_mall_latitude"
            :placeholder="$t('config_mall.placeholder.mall_latitude')"
          />
        </el-col>
      </el-form-item>
      <el-form-item
        :label="$t('config_mall.form.mall_phone')"
        prop="waterstore_mall_phone"
      >
        <el-input v-model="dataForm.waterstore_mall_phone" />
      </el-form-item>
      <el-form-item
        :label="$t('config_mall.form.mall_qq')"
        prop="waterstore_mall_qq"
      >
        <el-input v-model="dataForm.waterstore_mall_qq" />
      </el-form-item>
      <el-form-item>
        <el-button @click="cancel">{{ $t("app.button.cancel") }}</el-button>
        <el-button type="primary" @click="update">{{
          $t("app.button.confirm")
        }}</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
import { listMall, updateMall } from '@/api/config'

export default {
  name: 'ConfigMail',
  data() {
    return {
      dataForm: {
        waterstore_mall_name: '',
        waterstore_mall_address: '',
        waterstore_mall_phone: '',
        waterstore_mall_qq: '',
        waterstore_mall_longitude: '',
        waterstore_mall_latitude: ''
      },
      rules: {
        waterstore_mall_name: [
          { required: true, message: '不能为空', trigger: 'blur' }
        ],
        waterstore_mall_address: [
          { required: true, message: '不能为空', trigger: 'blur' }
        ],
        waterstore_mall_phone: [
          { required: true, message: '不能为空', trigger: 'blur' }
        ],
        waterstore_mall_qq: [
          { required: true, message: '不能为空', trigger: 'blur' }
        ]
      }
    }
  },
  created() {
    this.init()
  },
  methods: {
    init: function() {
      listMall().then((response) => {
        this.dataForm = response.data.data
      })
    },
    cancel() {
      this.init()
    },
    update() {
      this.$refs['dataForm'].validate((valid) => {
        if (!valid) {
          return false
        }
        this.doUpdate()
      })
    },
    doUpdate() {
      updateMall(this.dataForm)
        .then((response) => {
          this.$notify.success({
            title: '成功',
            message: '商场配置成功'
          })
        })
        .catch((response) => {
          this.$notify.error({
            title: '失败',
            message: response.data.errmsg
          })
        })
    }
  }
}
</script>
