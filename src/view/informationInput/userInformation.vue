<template>
  <Row>
    <Col span="24">
      <Card>
        <Row>
          <Form ref="formValidate" :model="formValidate" :rules="ruleValidate" :label-width="100">
            <FormItem label="头像">
              <avatar :img-url="imgUrl"></avatar>
              <div class="changeavatar">
                <Button style="margin-top: 5px;" @click="uploadHeadImg">修改头像</Button>
                <input
                  type="file"
                  accept="image/jpeg, image/jpg, image/png"
                  @change.stop="handleFile"
                  class="hiddenInput"
                >
              </div>
            </FormItem>
            <FormItem label="姓名" prop="username">
              <Input v-model="formValidate.username" style="width: 350px"></Input>
            </FormItem>
            <FormItem label="工号" prop="user_id">
              <Input v-model="formValidate.user_id" style="width: 350px"></Input>
            </FormItem>
            <FormItem label="部门" prop="dept_name">
              <Select v-model="formValidate.dept_name" style="width: 350px" filterable clearable>
                <Option-group label="行政中心">
                  <i-option value="0">人事部</i-option>
                  <i-option value="1">行政部</i-option>
                  <i-option value="2">财务部</i-option>
                </Option-group>
                <Option-group label="市场中心">
                  <i-option value="3">销售部</i-option>
                  <i-option value="4">策划部</i-option>
                  <i-option value="5">运营部</i-option>
                </Option-group>
                <Option-group label="客服中心">
                  <i-option value="6">售后服务部</i-option>
                  <i-option value="7">客户管理部</i-option>
                </Option-group>
                <Option-group label="生产中心">
                  <i-option value="8">生产部</i-option>
                  <i-option value="9">采购部</i-option>
                  <i-option value="10">仓储部</i-option>
                  <i-option value="11">物流部</i-option>
                </Option-group>
                <Option-group label="研发中心">
                  <i-option value="12">研发部</i-option>
                  <i-option value="13">测试部</i-option>
                  <i-option value="14">设计部</i-option>
                  <i-option value="15">产品部</i-option>
                </Option-group>
              </Select>
            </FormItem>
            <FormItem label="职位" prop="position">
              <Select v-model="formValidate.position" style="width: 350px" filterable clearable>
                <i-option value="0">Java工程师</i-option>
                <i-option value="1">测试工程师</i-option>
                <i-option value="2">产品经理</i-option>
                <i-option value="3">运营主管</i-option>
              </Select>
            </FormItem>
            <FormItem label="性别" prop="gender">&emsp;
              <RadioGroup v-model="formValidate.gender">
                <Radio label="男" value="1"></Radio>&emsp;
                <Radio label="女" value="0"></Radio>
              </RadioGroup>
            </FormItem>
            <FormItem>
              <Button type="primary" @click="handleSubmit('formValidate')">保存</Button>&nbsp;
              <Button @click="handleReset('formValidate')" style="margin-left: 8px">重置</Button>&nbsp;
            </FormItem>
          </Form>
        </Row>
      </Card>
    </Col>
  </Row>
</template>

<script>
import avatar from '../../components/avatar/avatar.vue'
export default {
  name: 'userInformation_page',
  components: { avatar },
  data () {
    return {
      formValidate: {
        username: '张三',
        user_id: '20191001',
        dept_name: '销售部',
        position: '初级顾问',
        gender: '1',
        imgUrl: '../../assets/images/tx.jpg'
      },
      ruleValidate: {
        gender: [
          { required: true, message: '请选择性别', trigger: 'change' }
        ]
      },
      formCustom: {
        old_passwd: '',
        new_passwd: '',
        passwdCheck: ''
      },
      imgUrl: 'https://cloud-minapp-22871.cloud.ifanrusercontent.com/1gZyi2P6x0yplHWi.jpg'
    }
  },
  methods: {
    handleSubmit (name) {
      this.$refs[name].validate((valid) => {
        if (valid) {
          this.$Message.success('保存成功!')
        } else {
          this.$Message.error('保存失败!')
        }
      })
    },
    handleReset (name) {
      this.$refs[name].resetFields()
    },
    // 修改头像
    uploadHeadImg () {
      // 点击修改头像，获取文档中 class=”hiddenInput” 的元素
      this.$el.querySelector('.hiddenInput').click()
    },
    handleFile (e) {
      let $target = e.target || e.srcElement
      let file = $target.files[0]
      let reader = new FileReader()
      reader.onload = data => {
        let res = data.target || data.srcElement
        // 修改页面和vuex中的图片路径
        this.imgUrl = res.result
      }
      reader.readAsDataURL(file)
    }
  }
}
</script>

<style scoped>
.replace {
  display: block;
  font-size: 28px;
  color: #fff;
  padding: 10px 10px;
  border: 1px solid #fff;
  border-radius: 30px;
}
.changeavatar {
  position: relative;
}
.hiddenInput {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  display: none;
}
</style>
