<template>
  <div class="root">
    <div class="left">
      <div  class="left1"><b style="font-size: 18px;color: #44f0ff">省份介绍:</b><br>
        {{province_introduced}}
      </div>
      <div  class="left1"><b style="font-size: 18px;color: #44f0ff">和我相关:</b><br>
        <b style="color: #44f0ff">省份:</b>{{province}}<br>
        <b style="color: #44f0ff">是否去过:</b>{{scheduled}}<br>
        <b style="color: #44f0ff">我的感受:</b>{{feel}}
      </div>
      <div  class="left1"><b style="font-size: 18px;color: #44f0ff">我的计划:</b><br>
        <b style="color: #44f0ff">省份:</b>{{provinceplan}}<br>
        <b style="color: #44f0ff">计划出发时间:</b>{{scheduletimeplan}}<br>
        <b style="color: #44f0ff">准备工作:</b>{{prepare}}
      </div>
    </div>
    <div class="middle">
      <div>
        <ve-map :settings="chartSettings"
                :tooltip-visible="false"
                :legend-visible="false"
                :tooltip="chartTooltip"
                :events="chartEvents"
                :data="chartData">
        </ve-map>
      </div>
    </div>
    <div class="right" >
      <div class="right1"><b style="font-size: 18px;color: #44f0ff">修改状态:{{clickCity}}</b><br><br>
        <b style="color: #44f0ff">状态将改变为'已到达'</b><br>
        <b style="color: #44f0ff">到达时间:</b><br><input v-model="arrivetime"><br>
        <b style="color: #44f0ff">感受:</b><br><input v-model="arrivefeel"><br><br>
        <button @click="changeState">提交</button>
      </div>
      <div class="right1"><b style="font-size: 18px;color: #44f0ff">添加计划: {{clickCity}}</b><br><br>
        <b style="color: #44f0ff">计划旅行时间:</b><br><input v-model="plantime"><br>
          <b style="color: #44f0ff">计划准备工作:</b><br><input v-model="planprepare"><br><br>
        <button @click="addPlan">提交</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  created: function () {
    var _this = this
    this.chartTooltip = {
      formatter: function (e) {
        // 添加省份介绍
        for (const province of _this.provinces) {
          if (province.indexOf(e.name) !== -1) {
            _this.province_introduced = province
          }
        }
        // 取出去过的感受
        _this.province = e.name
        _this.scheduled = '未去过'
        _this.feel = ''
        for (const visite of _this.alreadyvisited) {
          if (visite.province === e.name) {
            _this.province = visite.province
            _this.scheduled = visite.scheduled
            _this.feel = visite.feel
          }
        }

        // 该省份已经做了计划
        _this.provinceplan = e.name
        _this.scheduletimeplan = '无计划'
        _this.prepare = ''
        for (const plan of _this.visiteplan) {
          if (plan.provinceplan === e.name) {
            _this.provinceplan = plan.provinceplan
            _this.scheduletimeplan = plan.scheduletimeplan
            _this.prepare = plan.prepare
          }
        }
      }
    }
  },
  data () {
    var _this = this
    this.chartEvents = {
      click: function (e) {
        _this.clickCity = e.name
      }
    }
    return {
      plantime: '',
      planprepare: '',
      arrivetime: '',
      arrivefeel: '',
      provinceplan: '',
      scheduletimeplan: '',
      prepare: '',
      province: '',
      scheduled: '',
      feel: '',
      clickCity: '',
      province_introduced: '',
      chartSettings: {
        selectData: true,
        hoverAnimation: true
      },
      chartData: {
        columns: ['位置', '是否去过'],
        rows: [
          { '位置': '新疆', '是否去过': '去过' },
          { '位置': '吉林', '是否去过': '去过' },
          { '位置': '浙江', '是否去过': '去过' }
        ]
      },
      provinces: [
        '新疆维吾尔自治区面积166万平方千米',
        '西藏自治区面积122.8万平方千米',
        '内蒙古自治区面积118.3万平方千米 ',
        '青海省面积72.23万平方千米',
        '四川省面积48.14万平方千米 ',
        '黑龙江省面积47.3万平方千米 ',
        '甘肃省面积45.44万平方千米',
        '云南省面积38.33万平方千米',
        '广西壮族自治区面积23.6万平方千米 ',
        '湖南省面积21.18万平方千米',
        '陕西省面积20.56万平方千米 ',
        '河北省面积18.77万平方千米 ',
        '吉林省面积18.74万平方千米 ',
        '湖北省面积18.59万平方千米 ',
        '广东省面积18万平方千米 ',
        '贵州省面积17.6万平方千米 ',
        '江西省面积16.7万平方千米 ',
        '河南省面积16.7万平方千米',
        '山西省面积15.63万平方千米 ',
        '山东省面积15.38万平方千米',
        '辽宁省面积14.59万平方千米 ',
        '安徽省面积13.97万平方千米 ',
        '福建省面积12.13万平方千米 ',
        '江苏省面积10.26万平方千米 ',
        '浙江省面积10.2万平方千米',
        '重庆市面积8.23万平方千米',
        '宁夏回族自治区面积6.64万平方千米 ',
        '台湾省面积3.6万平方千米 ',
        '海南省面积3.4万平方千米 ',
        '北京市面积1.68万平方千米 ',
        '天津市面积1.13万平方千米',
        '上海市面积0.63万平方千米 ',
        '香港特别行政区面积1101平方千米 ',
        '澳门特别行政区面积32.8平方千米'
      ],
      alreadyvisited: [
        {
          province: '新疆',
          scheduled: '2019-10-11',
          feel: '辽阔国度'
        },
        {
          province: '吉林',
          scheduled: '2019-10-11',
          feel: '辽阔国度'
        },
        {
          province: '浙江',
          scheduled: '2019-10-11',
          feel: '辽阔国度'
        }
      ],
      visiteplan: [
        {
          provinceplan: '青海',
          scheduletimeplan: '2020-5-11',
          prepare: '携带皮划艇(泛舟青海湖)'
        },
        {
          provinceplan: '西藏',
          scheduletimeplan: '2020-10-11',
          prepare: '携带氧气罐'
        }
      ]
    }
  },
  methods: {
    changeState: function () {
      // 判断是否已经到达过该省份
      for (const visite of this.alreadyvisited) {
        if (visite.province === this.clickCity) {
          alert('已经去过省份,无法修改状态')
          return
        }
      }
      if (this.arrivetime.length === 0) {
        alert('请正确输入曾经到达该省时间,不能为空')
        return
      }
      if (this.arrivefeel.length === 0) {
        alert('请正确输入旅游该省感受,不能为空')
        return
      }
      // 修改状态  主要两个
      this.chartData.rows.push({ '位置': this.clickCity, '是否去过': '去过' })

      var visited = {
        province: this.clickCity,
        scheduled: this.arrivetime,
        feel: this.arrivefeel
      }
      this.alreadyvisited.push(visited)
      this.clickCity = ''
      this.arrivetime = ''
      this.arrivefeel = ''
      alert('修改状态成功')
    },
    addPlan: function () {
      for (const plan of this.visiteplan) {
        if (plan.provinceplan === this.clickCity) {
          alert('此省份计划已经存在')
          return
        }
      }

      if (this.plantime.length === 0) {
        alert('请正确输入计划时间,不能为空')
        return
      }
      if (this.planprepare.length === 0) {
        alert('请正确输入计划准备,不能为空')
        return
      }
      // 添加计划

      var plan = {
        provinceplan: this.clickCity,
        scheduletimeplan: this.plantime,
        prepare: this.planprepare
      }
      this.visiteplan.push(plan)
      this.clickCity = ''
      this.plantime = ''
      this.planprepare = ''
      alert('添加计划成功')
    }
  }
}
</script>

<style scoped>
  .root{
    width: 1200px;
    margin: 0 auto;
    height: 100%;
  }
  .left{
    float: left;
    width: 210px;
    height: 100%;
    color: white;
  }
  .left1{
    width: 200px;
    height: 180px;
    margin-top: 20px;
    padding: 10px;
    border: 1px solid #44f0ff;
    border-radius:5px;
    background: #000c23;
  }
  .right{
    float: right;
    position: relative;
    left: -10px;
    width: 210px;
    height: 100%;
  }
  .right1{
    width: 200px;
    height: 280px;
    margin-top: 30px;
    padding: 10px;
    border: 1px solid #44f0ff;
    border-radius:5px;
    background: #000c23;
    color: white;
  }
  .middle{
    float: left;
    width: 530px;
    padding: 100px;
  }
</style>
