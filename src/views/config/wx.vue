<template>
  <div class="app-container">
    <el-form
      ref="dataForm"
      :rules="rules"
      :model="dataForm"
      status-icon
      label-width="300px"
    >
      <el-tabs tab-position="left">
        <el-tab-pane :label="$t('config_wx.section.home')">
          <el-form-item
            :label="$t('config_wx.form.index_new')"
            prop="store_wx_index_new"
          >
            <el-input v-model="dataForm.store_wx_index_new" />
          </el-form-item>
          <el-form-item
            :label="$t('config_wx.form.index_hot')"
            prop="store_wx_index_hot"
          >
            <el-input v-model="dataForm.store_wx_index_hot" />
          </el-form-item>
          <el-form-item
            :label="$t('config_wx.form.index_brand')"
            prop="store_wx_index_brand"
          >
            <el-input v-model="dataForm.store_wx_index_brand" />
          </el-form-item>
          <el-form-item
            :label="$t('config_wx.form.index_topic')"
            prop="store_wx_index_topic"
          >
            <el-input v-model="dataForm.store_wx_index_topic" />
          </el-form-item>
          <el-form-item
            :label="$t('config_wx.form.catlog_list')"
            prop="store_wx_catlog_list"
          >
            <el-input v-model="dataForm.store_wx_catlog_list" />
          </el-form-item>
          <el-form-item
            :label="$t('config_wx.form.catlog_goods')"
            prop="store_wx_catlog_goods"
          >
            <el-input v-model="dataForm.store_wx_catlog_goods" />
          </el-form-item>
        </el-tab-pane>
        <el-tab-pane :label="$t('config_wx.section.other')">
          <el-form-item
            :label="$t('config_wx.form.share')"
            prop="store_wx_share"
          >
            <el-switch v-model="dataForm.store_wx_share" />
          </el-form-item>
        </el-tab-pane>
      </el-tabs>
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
import { listWx, updateWx } from '@/api/config'

export default {
  name: 'ConfigWx',
  data() {
    return {
      dataForm: {
        store_wx_index_new: 0,
        store_wx_index_hot: 0,
        store_wx_index_brand: 0,
        store_wx_index_topic: 0,
        store_wx_catlog_list: 0,
        store_wx_catlog_goods: 0,
        store_wx_share: false
      },
      rules: {
        store_wx_index_new: [
          { required: true, message: '不能为空', trigger: 'blur' }
        ],
        store_wx_index_hot: [
          { required: true, message: '不能为空', trigger: 'blur' }
        ],
        store_wx_index_brand: [
          { required: true, message: '不能为空', trigger: 'blur' }
        ],
        store_wx_index_topic: [
          { required: true, message: '不能为空', trigger: 'blur' }
        ],
        store_wx_catlog_list: [
          { required: true, message: '不能为空', trigger: 'blur' }
        ],
        store_wx_catlog_goods: [
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
      listWx().then((response) => {
        this.dataForm = response.data.data
        this.dataForm.store_wx_share = this.dataForm.store_wx_share === 'true'
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
      updateWx(this.dataForm)
        .then((response) => {
          this.$notify.success({
            title: '成功',
            message: '小程序配置成功'
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
