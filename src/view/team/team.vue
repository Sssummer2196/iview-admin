<template>
  <Card style="padding: 10px; width: 100%">
    <Row>
      <Col>
        <i-button style="float: right;" type="primary" shape="circle" icon="ios-search" @on-click="searchData"></i-button>
        <Input style="width:200px; float: right; margin-right: 10px;" placeholder="姓名" v-model="searchName" @on-blur="changeName" clearable></Input>
        <Select v-model="dept" placeholder="部门" style="width:200px; float: right; margin-right: 10px;" clearable>
          <Option-group label="行政中心">
            <Option v-for="item in deptList[0]" v-bind:key="item.value" :value="item.value">{{ item.label }}</Option>
          </Option-group>
          <Option-group label="市场中心">
            <Option v-for="item in deptList[1]" v-bind:key="item.value" :value="item.value">{{ item.label }}</Option>
          </Option-group>
        </Select>
      </Col>
    </Row>
    <Row style="width: 100%; background: #f0f0f0; height: auto; margin-top: 20px; line-height: 40px;">
      <Tag type="dot" v-for="item in labelList" v-bind:key="item.param" closable style="margin-left: 5px;" @on-close="clearParam(item.value, item.param)">{{item.label}}</Tag>
      <span style="color: #348EED; cursor: pointer; margin-left: 10px;" @click="clearAllParam" v-show="labelList.length !== 0">清空搜索条件</span>
    </Row>
    <Row>
      <Table :columns="columns1" :data="data1"></Table>
    </Row>
  </Card>
</template>

<script>
export default {
  name: 'team_page',
  watch: {
    dept: function () {
      if (this.dept > -1) {
        let deptName = ''
        this.deptList.forEach(x => {
          x.forEach(y => {
            if (y.value === this.dept) {
              deptName = y.label
            }
          })
        })
        let mark = 0
        this.labelList.forEach(x => {
          if (x.param === this.dept) {
            mark = 1
          }
        })
        if (mark === 0) {
          this.labelList.push({
            value: 0,
            label: deptName,
            param: this.dept
          })
        }
      }
    }
  },
  methods: {
    searchData () {

    },
    clearParam (type, param) {
      console.log(this.labelList)
      switch (type) {
        case 1:
          this.searchName = ''
          for (let item = 0; item < this.labelList.length; item++) {
            if (this.labelList[item].value === 1) {
              this.labelList.splice(item, 1)
              break
            }
          }
          break
        case 0:
          for (let item = 0; item < this.labelList.length; item++) {
            if (this.labelList[item].param === param) {
              this.labelList.splice(item, 1)
              break
            }
          }
          break
      }
    },
    clearAllParam () {
      this.labelList = []
    },
    changeName () {
      if (this.searchName.trim() === '') {
        return
      }
      let mark = 0
      this.labelList.forEach(x => {
        if (x.value === 1) {
          x.label = this.searchName.trim()
          x.param = this.searchName.trim()
          mark = 1
        }
      })
      if (mark === 0) {
        this.labelList.push({
          value: 1,
          label: this.searchName.trim(),
          param: this.searchName.trim()
        })
      }
    }
  },
  data () {
    return {
      searchName: '',
      dept: -1,
      deptList: [
        [
          {
            value: 0,
            label: '人事部'
          },
          {
            value: 1,
            label: '行政部'
          },
          {
            value: 2,
            label: '财务部'
          }
        ],
        [
          {
            value: 3,
            label: '销售部'
          },
          {
            value: 4,
            label: '策划部'
          },
          {
            value: 5,
            label: '运营部'
          }
        ]
      ],
      labelList: [],
      columns1: [
        {
          title: '姓名',
          key: 'name',
          align: 'center'
        },
        {
          title: '部门',
          key: 'dept',
          align: 'center'
        },
        {
          title: '职务',
          key: 'work',
          align: 'center'
        },
        {
          title: '性别',
          key: 'sex',
          align: 'center'
        },
        {
          title: '邮箱',
          key: 'email',
          align: 'center',
          width: 200
        },
        {
          title: '快捷操作',
          key: 'doSth',
          align: 'center'
        }
      ],
      data1: [
        {
          name: '王小明',
          dept: '市场中心',
          work: 'Java工程师',
          sex: '男',
          email: 'wangxiaoming@163.com'
        },
        {
          name: '顾大宝',
          dept: '市场中心',
          work: '测试工程师',
          sex: '女',
          email: 'gly5252@163.com'
        },
        {
          name: '韩业红',
          dept: '行政部',
          work: '运营',
          sex: '女',
          email: 'yeammhan@163.com'
        }
      ]
    }
  }
}
</script>
