<template>
  <Card style="padding: 10px; width: 100%">
    <Row>
      <Col>
        <i-button style="float: right;" type="primary" shape="circle" icon="ios-search" @on-click="searchData"></i-button>
        <Select placeholder="审批状态" style="width:100px; float: right; margin-right: 10px;" v-model="type" clearable>
          <i-option v-for="item in typeList" v-bind:key="item.value" :value="item.value">{{ item.label }}</i-option>
        </Select>
        <Date-picker type="daterange" @on-change="changeDate" placement="bottom-end" placeholder="选择日期" style="width: 200px; float: right; margin-right: 10px;"></Date-picker>
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
  name: 'task_page',
  watch: {
    type: function () {
      let mark = 0
      this.labelList.forEach(x => {
        if (x.param === this.type) {
          mark = 1
        }
      })
      if (mark === 1) {
        return
      }
      this.typeList.forEach(x => {
        if (x.value === this.type) {
          this.labelList.push({
            value: 1,
            label: x.label,
            param: x.value
          })
        }
      })
    },
    chooseDate: function () {
      console.log(this.chooseDate)
      if (this.chooseDate[0] === '' || this.chooseDate[1] === '') {
        return
      }
      let r = 0
      this.labelList.forEach(x => {
        if (x.value === 0) {
          r = 1
          x.label = this.chooseDate[0] + ' ~ ' + this.chooseDate[1]
          x.param = this.chooseDate
        }
      })
      if (r === 0) {
        this.labelList.push({
          value: 0,
          label: this.chooseDate[0] + ' ~ ' + this.chooseDate[1],
          param: this.chooseDate
        })
      }
    }
  },
  methods: {
    searchData () {

    },
    clearParam (type, param) {
      console.log(this.labelList)
      switch (type) {
        case 0:
          this.chooseDate = ['', '']
          for (let item = 0; item < this.labelList.length; item++) {
            if (this.labelList[item].value === 0) {
              this.labelList.splice(item, 1)
              break
            }
          }
          break
        case 1:
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
    changeDate (date) {
      this.chooseDate = date
    }
  },
  data () {
    return {
      count: 3,
      chooseDate: [],
      labelList: [],
      type: -1,
      typeList: [
        {
          value: 0,
          label: '未提交'
        },
        {
          value: 1,
          label: '正在审核'
        },
        {
          value: 2,
          label: '审核通过'
        }
      ],
      options2: {
        shortcuts: [
          {
            text: '最近一周',
            value () {
              const end = new Date()
              const start = new Date()
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 7)
              return [start, end]
            }
          },
          {
            text: '近一个月',
            value () {
              const end = new Date()
              const start = new Date()
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 30)
              return [start, end]
            }
          },
          {
            text: '近三个月',
            value () {
              const end = new Date()
              const start = new Date()
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 90)
              return [start, end]
            }
          }
        ]
      },
      columns1: [
        {
          title: '任务名称',
          key: 'taskName',
          align: 'center',
          fixed: 'left',
          width: 300
        },
        {
          title: '完成度',
          key: 'completeRate',
          align: 'center',
          width: 150
        },
        {
          title: '负责人',
          key: 'contactMan',
          align: 'center',
          width: 150
        },
        {
          title: '权重',
          key: 'rate',
          align: 'center',
          width: 150
        },
        {
          title: '发布时间',
          key: 'createTime',
          align: 'center',
          width: 150
        },
        {
          title: '截止时间',
          key: 'endTime',
          align: 'center',
          width: 150
        },
        {
          title: '更新时间',
          key: 'updateTime',
          align: 'center',
          width: 150
        },
        {
          title: '审批状态',
          key: 'auditStatus',
          align: 'center',
          fixed: 'right',
          width: 100
        },
        {
          title: '操作',
          key: 'doSth',
          align: 'center',
          fixed: 'right',
          width: 150
        }
      ],
      data1: [
        {
          taskName: '优化平台体验，促进用户注册产品',
          completeRate: '90%',
          contactMan: '韩业红',
          rate: '3',
          createTime: '2019-5-4 15:00',
          endTime: '2019-5-8 15:00',
          updateTime: '2019-5-5 12:42',
          auditStatus: '未提交'
        },
        {
          taskName: '优化平台体验，促进用户注册产品',
          completeRate: '90%',
          contactMan: '韩业红',
          rate: '3',
          createTime: '2019-5-4 15:00',
          endTime: '2019-5-8 15:00',
          updateTime: '2019-5-5 12:42',
          auditStatus: '未提交'
        },
        {
          taskName: '优化平台体验，促进用户注册产品',
          completeRate: '90%',
          contactMan: '韩业红',
          rate: '3',
          createTime: '2019-5-4 15:00',
          endTime: '2019-5-8 15:00',
          updateTime: '2019-5-5 12:42',
          auditStatus: '未提交'
        },
        {
          taskName: '优化平台体验，促进用户注册产品',
          completeRate: '90%',
          contactMan: '韩业红',
          rate: '3',
          createTime: '2019-5-4 15:00',
          endTime: '2019-5-8 15:00',
          updateTime: '2019-5-5 12:42',
          auditStatus: '未提交'
        }
      ]
    }
  }
}
</script>
