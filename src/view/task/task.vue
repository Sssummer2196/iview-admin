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
    <Row style="font-weight: bolder; width: 100%">
      <Col span="6" style="text-align: center">任务名称</Col>
      <Col span="2" style="text-align: center">完成度</Col>
      <Col span="2" style="text-align: center">负责人</Col>
      <Col span="1" style="text-align: center">权重</Col>
      <Col span="3" style="text-align: center">发布时间</Col>
      <Col span="3" style="text-align: center">截止时间</Col>
      <Col span="3" style="text-align: center">更新时间</Col>
      <Col span="2" style="text-align: center">审批状态</Col>
      <Col span="2" style="text-align: center">操作</Col>
    </Row>
    <Divider />
    <Row>
      <Tree :data="data5" :render="renderContent"></Tree>
    </Row>
  </Card>
</template>

<script>
export default {
  name: 'task_page',
  mounted: function () {
    let temp = JSON.parse(JSON.stringify(this.componentData))
    temp.render = this.componentData.render
    temp.completeRate = 20
    temp.strokeColor = this.getProcessColor(temp.completeRate)
    this.data5.push(temp)
    temp = JSON.parse(JSON.stringify(this.componentData))
    temp.render = this.componentData.render
    temp.completeRate = 60
    temp.strokeColor = this.getProcessColor(temp.completeRate)
    this.data5.push(temp)
    temp = JSON.parse(JSON.stringify(this.componentData))
    temp.render = this.componentData.render
    temp.completeRate = 88
    temp.strokeColor = this.getProcessColor(temp.completeRate)
    this.data5.push(temp)
    temp = JSON.parse(JSON.stringify(this.componentData))
    temp.render = this.componentData.render
    temp.completeRate = 100
    temp.strokeColor = this.getProcessColor(temp.completeRate)
    this.data5[1].children.push(temp)

    console.log(temp)
    console.log(this.componentData)
    console.log(this.data5)
  },
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
    getProcessColor (data) {
      if (data >= 0 && data < 50) {
        return '#ff0033'
      }
      if (data >= 50 && data < 80) {
        return '#ffff66'
      }
      if (data >= 80 && data < 100) {
        return '#00ffff'
      }
      return '#00ff66'
    },
    renderContent (h, { root, node, data }) {
      return h('span', {
        style: {
          display: 'inline-block',
          width: '100%'
        }
      }, [
        h('span', [
          h('Icon', {
            props: {
              type: 'ios-paper-outline'
            },
            style: {
              marginRight: '8px'
            }
          }),
          h('span', data.title)
        ]),
        h('span', {
          style: {
            display: 'inline-block',
            float: 'right',
            marginRight: '32px'
          }
        }, [
          h('Button', {
            props: Object.assign({}, this.buttonProps, {
              icon: 'ios-add'
            }),
            style: {
              marginRight: '8px'
            },
            on: {
              click: () => { this.append(data) }
            }
          }),
          h('Button', {
            props: Object.assign({}, this.buttonProps, {
              icon: 'ios-remove'
            }),
            on: {
              click: () => { this.remove(root, node, data) }
            }
          })
        ])
      ])
    },
    append (data) {
      const children = data.children || []
      children.push({
        title: 'appended node',
        expand: true
      })
      this.$set(data, 'children', children)
    },
    remove (root, node, data) {
      const parentKey = root.find(el => el === node).parent
      const parent = root.find(el => el.nodeKey === parentKey).node
      const index = parent.children.indexOf(data)
      parent.children.splice(index, 1)
    },
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
      componentData: {
        title: '优化平台体验，促进用户注册产品',
        taskName: '优化平台体验，促进用户注册产品',
        completeRate: 90,
        contactMan: '韩业红',
        rate: '50%',
        createTime: '2019-5-4 15:00',
        endTime: '2019-5-8 15:00',
        updateTime: '2019-5-5 12:42',
        auditStatus: '未提交',
        strokeColor: '',
        expand: false,
        doSth: false,
        render: (h, { root, node, data }) => {
          return h('div', {
            style: {
              display: 'inline-block',
              width: '100%'
            }
          }, [
            h('span', [
              h('Icon', {
                props: {
                  type: 'ios-bookmark-outline'
                },
                style: {
                  marginRight: '8px',
                  fontSize: '20px'
                }
              }),
              h('span', {
                style: {
                  cursor: 'pointer',
                  color: '#348EED',
                  width: '20%',
                  display: 'inline-block',
                  whiteSpace: 'normal'
                }
              }, data.taskName)
            ]),
            h('span', {
              style: {
                marginLeft: '10px',
                width: '10%',
                display: 'inline-block'
              }
            }, [
              h('Progress', {
                props: {
                  percent: data.completeRate,
                  strokeColor: data.strokeColor
                }
              })
            ]),
            h('span', {
              style: {
                marginLeft: '5px',
                width: '4%',
                display: 'inline-block'
              }
            }, data.contactMan),
            h('span', {
              style: {
                marginLeft: '30px',
                width: '3%',
                display: 'inline-block'
              }
            }, data.rate),
            h('span', {
              style: {
                marginLeft: '2.5%',
                width: '10%',
                display: 'inline-block'
              }
            }, data.createTime),
            h('span', {
              style: {
                marginLeft: '2.5%',
                width: '10%',
                display: 'inline-block'
              }
            }, data.endTime),
            h('span', {
              style: {
                marginLeft: '2.5%',
                width: '10%',
                display: 'inline-block'
              }
            }, data.updateTime),
            h('span', {
              style: {
                marginLeft: '2.5%',
                width: '5%',
                display: 'inline-block'
              }
            }, data.auditStatus),
            h('span', {
              style: {
                float: 'right',
                marginRight: '1.5%',
                marginTop: '-10px',
                width: '5%',
                color: '#00CC00',
                fontSize: '20px',
                cursor: 'pointer',
                display: 'inline-block'
              }
            }, [
              h('span', {
                on: {
                  click: () => {
                    data.doSth = !data.doSth
                  }
                }
              }, [
                h('Icon', {
                  props: {
                    type: 'md-arrow-dropdown'
                  }
                })
              ]),
              h('DropdownMenu', {
                props: {
                  slot: 'list'
                },
                directives: [
                  {
                    name: 'show',
                    value: data.doSth
                  }
                ]
              }, [
                h('DropdownItem', {
                  style: {
                    width: '60px'
                  }
                }, '删除'),
                h('DropdownItem', {
                  style: {
                    width: '60px'
                  }
                }, '提交'),
                h('DropdownItem', {
                  style: {
                    width: '60px'
                  }
                }, '修改'),
                h('DropdownItem', {
                  style: {
                    width: '60px'
                  }
                }, '新增')
              ])
            ]),
            h('Divider')
          ])
        },
        children: []
      },
      data5: [],
      buttonProps: {
        type: 'default',
        size: 'small'
      },
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
          width: 300,
          render: (h, params) => {
            return h('div', [
              h('Icon', {
                props: {
                  type: 'person'
                }
              }),
              h('strong', params.row.taskName)
            ])
          }
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
      ]
    }
  }
}
</script>
