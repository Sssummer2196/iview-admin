<template>
  <Row style="padding: 40px;">
    <Row>
      <span style="color: #0099ff; font-size: 28px; font-weight: bolder">2018年底绩效评分</span>
    </Row>
    <Row>
      <Col style="width: 150px; height: 150px; border: 1px #ccc solid; background: #ccc; float: left">
        <Col style="width: 90%; height:90%; margin: 5%; border: 1px #fff solid; background: #fff;">
          <Col style="width: 96%; height:96%; margin: 2%; border: 1px #fff solid;">
            <img style="width: 100%; height:100%;" src="../../assets/images/tx.jpg" />
          </Col>
        </Col>
      </Col>
      <Col style="float: left; margin-left: 10px;">
        <Row style="color: #58cc00; font-size: 28px; margin: 10px;">
          <span>韩业黄</span>
        </Row>
        <Row style="margin: 10px; color: #797979;">
          <span>研发部 | Java工程师</span>
        </Row>
        <Row style="margin: 10px; color: #797979;">
          <span>gly5252@163.com</span>
        </Row>
      </Col>
    </Row>
    <Card style="margin-top: 10px; padding: 10px; height: 90px;">
      <Col style="float: left; width: 10%">
        <span style="font-size: 20px;">评分流程</span>
      </Col>
      <Col style="float: left; width: 90%">
        <Steps :current="0" style="float: left; margin-left: 10%; width: 90%;">
          <Step title="自评"></Step>
          <Step title="同级评分"></Step>
          <Step title="上级评分"></Step>
          <Step title="审核"></Step>
          <Step title="结束"></Step>
        </Steps>
      </Col>
    </Card>
    <Row style="margin-top: 20px;">
      <Collapse active-key="1" simple>
        <Panel key="1">
          工作态度
          <Row slot="content">
            <Row>
              <Col style="width: 60%; float: left">1、表现热情向上的精神状态、不向外倾诉工作上的不满</Col>
              <Select style="width: 200px; float: right" clearable>
                <Option v-for="item in selectOptions" :key="item.value" :value="item.value">{{item.label}}</Option>
              </Select>
            </Row>
            <Row style="margin-top: 10px;">
              <Col style="width: 60%; float: left">2、审查自己的能力，并学习新的行业知识、职业技能</Col>
              <Select style="width: 200px; float: right" clearable>
                <Option v-for="item in selectOptions" :key="item.value" :value="item.value">{{item.label}}</Option>
              </Select>
            </Row>
            <Row style="margin-top: 10px;">
              <Col style="width: 60%; float: left">3、虚心地听取他人建议、意见并可以改正自己的缺点</Col>
              <Select style="width: 200px; float: right" clearable>
                <Option v-for="item in selectOptions" :key="item.value" :value="item.value">{{item.label}}</Option>
              </Select>
            </Row>
            <Row style="margin-top: 10px;">
              <Col style="width: 60%; float: left">4、责任感强，确实完成交付的工作</Col>
              <Select style="width: 200px; float: right" clearable>
                <Option v-for="item in selectOptions" :key="item.value" :value="item.value">{{item.label}}</Option>
              </Select>
            </Row>
          </Row>
        </Panel>
        <Panel key="2">
          重要任务
          <Row slot="content">
            <Row>
              <Col style="width: 60%; float: left">1、弹幕系统用户调研，形成用户调研报告</Col>
              <Select style="width: 200px; float: right" clearable>
                <Option v-for="item in selectOptions" :key="item.value" :value="item.value">{{item.label}}</Option>
              </Select>
            </Row>
            <Row style="margin-top: 10px;">
              <Col style="width: 60%; float: left">2、明确弹幕系统解决方案，快速迭代，持续完善</Col>
              <Select style="width: 200px; float: right" clearable>
                <Option v-for="item in selectOptions" :key="item.value" :value="item.value">{{item.label}}</Option>
              </Select>
            </Row>
          </Row>
        </Panel>
      </Collapse>
    </Row>
    <Row style="margin-top: 10px;">
      <Button style="float: right" type="primary" shape="circle" size="large">提交</Button>
    </Row>
  </Row>
</template>

<script>
export default {
  name: 'mark_page',
  data () {
    return {
      selectOptions: [
        {
          value: 0,
          label: '非常优秀'
        },
        {
          value: 1,
          label: '比较优秀'
        },
        {
          value: 2,
          label: '一般水平'
        },
        {
          value: 3,
          label: '需要提高'
        }
      ]
    }
  }
}
</script>
