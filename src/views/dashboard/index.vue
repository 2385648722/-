<template>
  <el-tabs class="pane" type="border-card" v-model="activeName" @tab-click="handleClick">
    <el-tab-pane label="统计图表" name="first">
      <div>
        <div class="tatle-a">2023年汇总</div>
        <el-form>
          <el-form-item label="年份选择：">
            <el-select v-model="form.region" size="small" placeholder="请选择年份">
              <el-option label="2023年" value="shanghai"></el-option>
              <el-option label="2022年" value="beijing"></el-option>
            </el-select>
            <el-button class="buttoms" type="primary" size="small">确定</el-button>
          </el-form-item>
        </el-form>
        <!-- echarts图 -->
        <div class="echarts">
          <div id="main" class="left-echarts" />
          <div class="charts-box" id="aster">
          </div>
        </div>

        <!-- echarts检测项目汇总 -->
        <div>
          <div class="echarts-i">
            <div class="table-title">
              检测项目汇总
            </div>

            <div class="echartsb">
              <div id="mains" class="left-echartsb"></div>
              <div class="right-echarte">
                <div class="center-echartsb" id="chart"></div>
                <div id="chartsr" class="right-echartsb"></div>
              </div>
            </div>

          </div>
        </div>

      </div>

    </el-tab-pane>
    <el-tab-pane class="pane" label="数据汇总" name="second">
      <!-- 数据汇总表格 -->
      <div class="table">
        <div class="table-title">
          数据汇总
        </div>
        <div class="scelet">
          <el-form :inline="true">
            <el-form-item label="公司：">
              <el-select v-model="form.region2" size="small" placeholder="请选择公司">
                <el-option label="襄阳" value="shanghai"></el-option>
                <el-option label="黄石" value="beijing2"></el-option>
                <el-option label="深圳" value="beijing3"></el-option>
                <el-option label="广州" value="beijing4"></el-option>
                <el-option label="北京" value="beijing5"></el-option>
                <el-option label="上海" value="beijing6"></el-option>
              </el-select>
            </el-form-item>
            <el-form-item label="供应商：">
              <el-input placeholder="输入供应商名称" v-model="input" size="small" clearable>
              </el-input>
            </el-form-item>

            <el-form-item label="供应商编号：">
              <el-input placeholder="输入供应商编号" v-model="input1" size="small" clearable>
              </el-input>
            </el-form-item>
            <el-form-item label="项目：">
              <el-input placeholder="输入项目" v-model="input17" size="small" clearable>
              </el-input>
            </el-form-item>
            <!-- <el-form-item label="结论：">
                <el-input placeholder="输入结论" v-model="input31" size="small" clearable>
                </el-input>
              </el-form-item> -->
            <el-form-item label="检测人：">
              <el-input placeholder="输入检测人员" v-model="input172" size="small" clearable>
              </el-input>
            </el-form-item>

            <el-form-item label="结论：">
              <el-select v-model="form.region3" size="small" placeholder="请选择结论">
                <el-option label="合格" value="shanghai"></el-option>
                <el-option label="不合格" value="beijing2"></el-option>

              </el-select>
            </el-form-item>

            <el-form-item label="年份：">
              <el-date-picker v-model="value1" size="small" type="daterange" range-separator="至" start-placeholder="开始日期"
                end-placeholder="结束日期"></el-date-picker>
              <el-button type="primary" size="small">确定</el-button>
            </el-form-item>
          </el-form>

        </div>
        <el-table :data="tableDatas" height="840px" border style="width: 100%" align="center">
          <el-table-column prop="date" label="日期" align="center" />
          <el-table-column prop="namesr" label="分公司" align="center" />
          <el-table-column prop="supplier" label="供应商" align="center" />
          <el-table-column prop="supplierNumber" label="供应商编号" align="center" />
          <el-table-column prop="incoming" label="来料说明" align="center" />
          <el-table-column prop="number" label="数量" align="center" />
          <el-table-column prop="detectionNumber" label="检测项目" align="center" />

          <el-table-column prop="DetectionValue" label="检测值" align="center">
            <template slot-scope="scope">
              <span :style="{ color: scope.row.DetectionValue >= 80 ? 'red' : 'black' }">{{ scope.row.DetectionValue
              }}</span>
            </template>
          </el-table-column>
          <el-table-column prop="standard" label="内控标准" align="center" />
          <el-table-column prop="ReInspection" label="复检" align="center" />
          <el-table-column prop="result" label="结论" align="center">
            <template slot-scope="scope">
              <span :style="{ color: scope.row.result === '不合格' ? 'red' : 'black' }">{{ scope.row.result }}</span>
            </template>
          </el-table-column>
          <el-table-column prop="man" label="检测人员" align="center" />
          <el-table-column label="详情" align="center" />


        </el-table>
      </div>
    </el-tab-pane>
  </el-tabs>
</template>

<script>
import * as echarts from 'echarts'

export default {


  data() {
    return {
      activeName: 'second',
      value1: '',
      value2: '',
      input: '',
      input1: '',
      input2: '',
      input3: '',
      input17: '',
      input172: '',
      input31: '',
      input13: '',

      form: {
        region: '',
        region1: '',
        region2: '',
        region3: '',
        region4: '',
        region5: '',
        region6: '',
        type: [],
        resource: '',
        desc: ''
      },

      tableData: [{
        date: '2023-01',
        names: '深圳市绿诗源生物技术有限公司',
        namesa: '广西供应商',
        supplier: 'A',
        supplierNumber: 'BQ123456',
        incoming: '鸡尾虾',
        number: '10万',
        detectionNumber: '孔雀石绿',

        DetectionValue: '<80',
        standard: '80',
        ReInspection: '/',
        result: '合格',
        man: '邱'

      }, {
        date: '2023-02',
        names: '湖北有限公司',
        supplier: 'B',
        supplierNumber: 'BQ123456',
        incoming: '大白菜',
        number: '10万',
        detectionNumber: '克百威',
        DetectionValue: '>=80',
        standard: '80',
        ReInspection: '不合格',
        result: '不合格',
        man: '邱'

      },
      {
        date: '2023-03',
        names: '黄石有限公司',
        supplier: 'C',
        supplierNumber: 'BQ123456',
        incoming: '鸡尾虾',
        number: '10万',
        detectionNumber: '氯霉素',
        DetectionValue: '<80',
        standard: '80',
        ReInspection: '/',
        result: '合格',
        man: '邱'

      },
      {
        date: '2023-04',
        names: '北京有限公司',
        supplier: 'D',
        supplierNumber: 'BQ122222',
        incoming: '鸡尾虾',
        number: '10万',
        detectionNumber: '甲醇',
        DetectionValue: '<80',
        standard: '80',
        ReInspection: '/',
        result: '合格',
        man: '邱'

      },
      {
        date: '2023-05',
        names: '上海有限公司',
        supplier: 'E',
        supplierNumber: 'BQ122222',
        incoming: '鸡尾虾',
        number: '10万',
        detectionNumber: '磺胺类',
        DetectionValue: '<80',
        standard: '80',
        ReInspection: '/',
        result: '合格',
        man: '邱'

      },
      {
        date: '2023-06',
        names: '东莞有限公司',
        supplier: 'F',
        supplierNumber: 'BQ122222',
        incoming: '猪肉',
        number: '10万',
        detectionNumber: '瘦肉精',
        DetectionValue: '<80',
        standard: '80',
        ReInspection: '/',
        result: '合格',
        man: '邱'

      },
      {
        date: '2023-07',
        names: '天津有限公司',
        supplier: 'F',
        supplierNumber: 'BQ122222',
        incoming: '猪肉',
        number: '10万',
        detectionNumber: '瘦肉精',
        DetectionValue: '<80',
        standard: '80',
        ReInspection: '/',
        result: '合格',
        man: '邱'

      },
      {
        date: '2023-08',
        names: '网纽有限公司',
        supplier: 'F',
        supplierNumber: 'BQ122222',
        incoming: '猪肉',
        number: '10万',
        detectionNumber: '瘦肉精',
        DetectionValue: '<80',
        standard: '80',
        ReInspection: '/',
        result: '合格',
        man: '邱'

      },
      {
        date: '2023-09',
        names: '全湖有限公司',
        supplier: 'F',
        supplierNumber: 'BQ122222',
        incoming: '猪肉',
        number: '10万',
        detectionNumber: '瘦肉精',
        DetectionValue: '<80',
        standard: '80',
        ReInspection: '/',
        result: '合格',
        man: '邱'

      },
      {
        date: '2023-10',
        names: '胜庆有限公司',
        supplier: 'F',
        supplierNumber: 'BQ122222',
        incoming: '猪肉',
        number: '10万',
        detectionNumber: '瘦肉精',
        DetectionValue: '<80',
        standard: '80',
        ReInspection: '/',
        result: '合格',
        man: '邱'

      },
      {
        date: '2023-11',
        names: '速圣有限公司',
        supplier: 'F',
        supplierNumber: 'BQ122222',
        incoming: '猪肉',
        number: '10万',
        detectionNumber: '瘦肉精',
        DetectionValue: '<80',
        standard: '80',
        ReInspection: '/',
        result: '合格',
        man: '邱'

      },
      {
        date: '2023-12  ',
        names: '伦田有限公司',
        supplier: 'F',
        supplierNumber: 'BQ122222',
        incoming: '猪肉',
        number: '10万',
        detectionNumber: '瘦肉精',
        DetectionValue: '<80',
        standard: '80',
        ReInspection: '/',
        result: '合格',
        man: '邱'

      },
      ],
      // 汇总表格
      tableDatas: [{
        date: '2023-01-01',
        namesr: '深圳市绿诗源生物技术有限公司',
        supplier: 'A',
        supplierNumber: 'BQ123456',
        incoming: '鸡尾虾',
        number: '10万',
        detectionNumber: '孔雀石绿',
        DetectionValue: '<80',
        standard: '80',
        ReInspection: '/',
        result: '合格',
        man: '邱'

      }, {
        date: '2023-01-02',
        namesr: '深圳市绿诗源生物技术有限公司',
        supplier: 'B',
        supplierNumber: 'BQ123456',
        incoming: '大白菜',
        number: '10万',
        detectionNumber: '克百威',
        DetectionValue: '180',
        standard: '80',
        ReInspection: '150',
        result: '不合格',
        man: '邱'

      },
      {
        date: '2023-01-03',
        namesr: '深圳市绿诗源生物技术有限公司',
        supplier: 'C',
        supplierNumber: 'BQ123456',
        incoming: '鸡尾虾',
        number: '10万',
        detectionNumber: '氯霉素',
        DetectionValue: '<80',
        standard: '80',
        ReInspection: '/',
        result: '合格',
        man: '邱'

      },
      {
        date: '2023-01-04',
        namesr: '深圳市绿诗源生物技术有限公司',
        supplier: 'D',
        supplierNumber: 'BQ122222',
        incoming: '鸡尾虾',
        number: '10万',
        detectionNumber: '甲醇',
        DetectionValue: '<80',
        standard: '80',
        ReInspection: '/',
        result: '合格',
        man: '邱'

      },
      {
        date: '2023-01-05',
        namesr: '深圳市绿诗源生物技术有限公司',
        supplier: 'E',
        supplierNumber: 'BQ122222',
        incoming: '鸡尾虾',
        number: '10万',
        detectionNumber: '磺胺类',
        DetectionValue: '<80',
        standard: '80',
        ReInspection: '/',
        result: '合格',
        man: '邱'

      },
      {
        date: '2023-01-06',
        namesr: '深圳市绿诗源生物技术有限公司',
        supplier: 'F',
        supplierNumber: 'BQ122222',
        incoming: '猪肉',
        number: '10万',
        detectionNumber: '瘦肉精',
        DetectionValue: '<80',
        standard: '80',
        ReInspection: '/',
        result: '合格',
        man: '邱'
      },
      {
        date: '2023-01-07',
        namesr: '深圳市绿诗源生物技术有限公司',
        supplier: 'F',
        supplierNumber: 'BQ122222',
        incoming: '猪肉',
        number: '10万',
        detectionNumber: '瘦肉精',
        DetectionValue: '<80',
        standard: '80',
        ReInspection: '/',
        result: '合格',
        man: '邱'
      },
      {
        date: '2023-01-08',
        namesr: '深圳市绿诗源生物技术有限公司',
        supplier: 'F',
        supplierNumber: 'BQ122222',
        incoming: '猪肉',
        number: '10万',
        detectionNumber: '瘦肉精',
        DetectionValue: '<80',
        standard: '80',
        ReInspection: '/',
        result: '合格',
        man: '邱'
      },
      {
        date: '2023-01-09',
        namesr: '深圳市绿诗源生物技术有限公司',
        supplier: 'F',
        supplierNumber: 'BQ122222',
        incoming: '猪肉',
        number: '10万',
        detectionNumber: '瘦肉精',
        DetectionValue: '<80',
        standard: '80',
        ReInspection: '/',
        result: '合格',
        man: '邱'
      },
      {
        date: '2023-01-10',
        namesr: '深圳市绿诗源生物技术有限公司',
        supplier: 'F',
        supplierNumber: 'BQ122222',
        incoming: '猪肉',
        number: '10万',
        detectionNumber: '瘦肉精',
        DetectionValue: '<80',
        standard: '80',
        ReInspection: '/',
        result: '合格',
        man: '邱'
      },
      {
        date: '2023-01-11',
        namesr: '深圳市绿诗源生物技术有限公司',
        supplier: 'F',
        supplierNumber: 'BQ122222',
        incoming: '猪肉',
        number: '10万',
        detectionNumber: '瘦肉精',
        DetectionValue: '<80',
        standard: '80',
        ReInspection: '/',
        result: '合格',
        man: '邱'
      },
      {
        date: '2023-01-12',
        namesr: '深圳市绿诗源生物技术有限公司',
        supplier: 'F',
        supplierNumber: 'BQ122222',
        incoming: '猪肉',
        number: '10万',
        detectionNumber: '瘦肉精',
        DetectionValue: '<80',
        standard: '80',
        ReInspection: '/',
        result: '合格',
        man: '邱'
      },
      {
        date: '2023-01-13',
        namesr: '深圳市绿诗源生物技术有限公司',
        supplier: 'F',
        supplierNumber: 'BQ122222',
        incoming: '猪肉',
        number: '10万',
        detectionNumber: '瘦肉精',
        DetectionValue: '<80',
        standard: '80',
        ReInspection: '/',
        result: '合格',
        man: '邱'
      },
      ],
    }
  },
  mounted() {
    this.renderChart()
    this.renderChart1()


    this.renderCharts()
    this.renderChartr();
    this.renderChartrs()

  },
  methods: {
    handleClick(tab, event) {
      console.log(tab, event);
    },
    // 柱状图
    renderChart() {
      const chartDom = document.getElementById('main')
      const myChart = echarts.init(chartDom)

      const option = {
        title: {
          text: '分公司检验数据年度汇总'
        },
        tooltip: {
          trigger: 'axis',
          axisPointer: {
            type: 'shadow'
          }
        },
        legend: {
          right: 10 // 设置标签靠右对齐
        },
        grid: {
          left: '3%',
          right: '5%',
          bottom: '3%',
          containLabel: true
        },
        xAxis: {
          type: 'value',
          boundaryGap: [0, 0.01]
          // boundaryGap: ['0','20%',]
        },
        yAxis: {
          type: 'category',
          data: ['新沂大地', '沐阳益客', '江苏益客', '徐州益客', '徐州润客', '济宁众客', '菏泽益舟', '益客产业园', '众客产业园', '众客金泽', '山东众客', '众客恒泰',]
        },
        series: [
          {
            name: '合格',
            type: 'bar',
            stack: 'combined',
            data: [50, 68, 79, 102, 178, 221, 254, 265, 278, 290, 300, 321, 340],
            label: {
              show: true,
              position: 'inside',
              formatter: '{c}'
            }
          },
          {
            name: '不合格',
            type: 'bar',
            stack: 'combined',
            data: [3, 6, 5, 10, 16, 20, 17, 19, 23, 32, 27, 16, 10],
            label: {
              show: true,
              position: 'inside',
              formatter: '{c}'
            }
          },
          {
            // name: '合格率%',
            type: 'bar',
            stack: 'combined',
            // data: [59, 1, 102, 158, 245, 335],
            data: [10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10, 10],
            label: {
              show: true,
              borderType: 'solid',
              // position: 'right',
              formatter: function (value) {
                // let arr = []
                // let item = value.value + '%'
                // arr.push(item)
                // console.log(arr,value.dataIndex,'-----------------log');
                // return arr
                let arr = ["94%", "92%", "94%", "91%", "92%", "92%", "94%", "93%", "92%", "90%", "92%", "95%"]

                return arr[value.dataIndex]
              }
              ,
            },
            itemStyle: {
              color: 'rgba(255,255,255,0)'
            },
            labelLayout(params) {
              return {
                x: params.rect.x + 10,
                y: params.rect.y + params.rect.height / 2,
                verticalAlign: 'middle',
                align: 'left',

              }
            },
            tooltip: {
              valueFormatter: (value) => null
            }
          }
        ]
      }

      option && myChart.setOption(option)
    },
    // 二图
    renderChart1() {
      const chartDom1 = document.getElementById('aster')
      const myChart1 = echarts.init(chartDom1)

      const option = {
        title: {
          text: '众客恒泰-月度分析',
          left: 'center',
          textStyle: {
            color: '#333',

          },

        },

        tooltip: {
          trigger: 'axis',
          axisPointer: {
            type: 'shadow'
          }
        },
        xAxis: {
          data: ['1月', '2月', '3月', '4月', '5月', '6月', '7月', '8月', '9月', '10月', '11月', '12月']
        },
        yAxis: {},
        series: [
          {
            data: [10, 22, 28, 43, 49, 70, 67, 65, 45, 69, 32, 56],
            type: 'bar',
            stack: 'x',
            label: {
              show: true, // 显示标签
              position: 'inside' // 标签位置，可以根据需要调整
            },

          },
          {
            data: [5, 4, 3, 5, 10, 20, 13, 14, 5, 10, 19, 23],
            type: 'bar',
            stack: 'x',
            label: {
              show: true, // 显示标签
              position: 'inside' // 标签位置，可以根据需要调整
            }

          },
          {
            // name: '总数量',
            type: 'bar',
            stack: 'x',
            data: [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,],
            // data: [58, 98, 127, 170, 178, 203, 264, 303,308,320, 298, 321,],
            label: {
              show: true,
              position: 'inside',
              formatter: function (value) {
                let arr =[15, 26, 31, 48, 59, 90, 80, 79,50,79, 51, 79,]
                return arr[value.dataIndex]
              }
            },
            itemStyle: {
              color: 'rgba(255,255,255,0)'
            },
            labelLayout(params) {
              return {
                x: params.rect.x + 10,
                // y: params.rect.y + params.rect.height / 2,
                y: params.rect.y + -14,
                verticalAlign: 'middle',
                align: 'top',
              }
            },
            tooltip: {
              valueFormatter: (value) => null
            }
          }
        ]
      };

      myChart1.setOption(option); // 应用配置项并渲染图表
    },
    // 供应商柱状图
    renderCharts() {
      const chartDom = document.getElementById('mains')
      const myChart = echarts.init(chartDom)
      const option = {
        title: {
          text: '供应商年度质量分析'
        },
        tooltip: {
          trigger: 'axis',
          axisPointer: {
            type: 'shadow'
          }
        },
        legend: {
          right: 10 // 设置标签靠右对齐
        },
        grid: {
          left: '3%',
          right: '5%',
          bottom: '3%',
          containLabel: true
        },
        xAxis: {
          type: 'value',
          boundaryGap: [0, 0.01]
        },
        yAxis: {
          type: 'category',
          data: ['供应商1', '供应商2', '供应商3', '供应商4', '供应商5', '供应商6', '供应商7', '供应商8', '供应商9', '供应商10']
        },
        series: [
          {
            name: '合格',
            type: 'bar',
            stack: 'combined',
            data: [50, 80, 100, 152, 178, 189, 210, 239, 260, 279, 298, 321,],
            label: {
              show: true,
              position: 'inside',
              formatter: '{c}'
            }
          },
          {
            name: '不合格',
            type: 'bar',
            stack: 'combined',
            data: [8, 18, 27, 18, 25, 54, 64, 68, 51, 57, 68, 80,],
            label: {
              show: true,
              position: 'inside',
              formatter: '{c}'
            }
          }, {
            // name: '总数量',
            type: 'bar',
            stack: 'combined',
            data: [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,],
            // data: [58, 98, 127, 170, 178, 203, 264, 303,308,320, 298, 321,],
            label: {
              show: true,
              position: 'inside',
              formatter: function (value) {
                let arr =[58, 98, 127, 170, 203, 243, 274, 307,311,336, 336, 401,]
                return arr[value.dataIndex]
              }
            },
            itemStyle: {
              color: 'rgba(255,255,255,0)'
            },
            labelLayout(params) {
              return {
                x: params.rect.x + 15,
                y: params.rect.y + params.rect.height / 2,
                verticalAlign: 'middle',
                align: 'left',
              }
            },
            tooltip: {
              valueFormatter: (value) => null
            }
          }
        ]
      }

      option && myChart.setOption(option)
    },
    // 供应商饼状图中
    renderChartr() {
      const chartDom = document.getElementById('chart');
      const myChart = echarts.init(chartDom);
      const option = {
        title: {
          text: '前十供应商不合格率',
          left: 'center'
        },
        tooltip: {
          trigger: 'item',
          formatter: '{a} <br/>{b}: {d}%'
        },
        legend: {
          orient: 'horizontal',
          top: 'bottom',
          data: ['供应商1', '供应商2', '供应商3', '供应商4', '供应商5', '供应商6', '供应商7', '供应商8', '供应商9', '供应商10']
        },
        color: ['#c23531', '#2f4554', '#61a0a8', '#d48265', '#91c7ae', '#749f83', '#ca8622', '#bda29a', '#91cc75', '#546570'],
        series: [
          {
            name: '数据',
            type: 'pie',
            radius: '50%',
            center: ['50%', '40%'],
            data: [
              { value: 33, name: '供应商1' },
              { value: 310, name: '供应商2' },
              { value: 234, name: '供应商3' },
              { value: 135, name: '供应商4' },
              { value: 90, name: '供应商5' },
              { value: 231, name: '供应商6' },
              { value: 245, name: '供应商7' },
              { value: 248, name: '供应商8' },
              { value: 138, name: '供应商9' },
              { value: 154, name: '供应商10' },
            ],
            emphasis: {
              itemStyle: {
                shadowBlur: 10,
                shadowOffsetX: 0,
                shadowColor: 'rgba(0, 0, 0, 0.5)'
              }
            },
            label: {
              show: true,
              formatter: '{d}%'
            },
            labelLine: {
              show: true
            }
          }
        ]
      };

      option && myChart.setOption(option);
    },
    // 供应商饼状图右
    renderChartrs() {
      const chartDom = document.getElementById('chartsr');
      const myChart = echarts.init(chartDom);
      const option = {
        title: {
          text: '前十不合格项目数',
          left: 'center'
        },
        tooltip: {
          trigger: 'item',
          formatter: '{a} <br/>{b}: {d}%'
        },
        legend: {
          orient: 'horizontal',
          top: 'bottom',
          data: ['甲硝唑', '氯霉素', '喹诺酮', '磺胺', '四环素', '呋喃唑酮', '呋喃它酮', '氟苯尼考', '替米考星', '喹乙醇代谢物']
        },
        color: ['#c23531', '#99a8ff', '#61a0a8', '#d48265', '#91c7ae', '#749f83', '#ca8622', '#bda29a', '#91cc75', '#f2781e'],
        series: [
          {
            name: '数据',
            type: 'pie',
            radius: '50%',
            center: ['50%', '40%'],
            data: [
              { value: 33, name: '甲硝唑' },
              { value: 310, name: '氯霉素' },
              { value: 234, name: '喹诺酮' },
              { value: 135, name: '磺胺' },
              { value: 90, name: '四环素' },
              { value: 231, name: '呋喃唑酮' },
              { value: 245, name: '呋喃它酮' },
              { value: 248, name: '氟苯尼考' },
              { value: 138, name: '替米考星' },
              { value: 154, name: '喹乙醇代谢物' },
            ],
            emphasis: {
              itemStyle: {
                shadowBlur: 10,
                shadowOffsetX: 0,
                shadowColor: 'rgba(0, 0, 0, 0.5)'
              }
            },
            label: {
              show: true,
              formatter: '{d}%'
            },
            labelLine: {
              show: true
            }
          }
        ]
      };

      option && myChart.setOption(option);
    },




  }
}
</script>

<style scoped lang="scss" scoped>
.pane {
  font-size: 30px;
  overflow: hidden !important;
}



.tatle-a {
  margin-top: 10px;
  text-align: center;
  font-size: 30px;
  color: black;
  font-weight: 700;
}

.buttoms {
  margin-left: 2px;
}

.echarts {
  width: 100%;
  display: flex;
  justify-content: space-between;

  .left-echarts {
    width: 736.5px;
    height: 300px;
  }


}

.charts-box {
  width: 736.5px;
  height: 300px;
}

.chart {
  width: 100%;
  height: 100%;
}

// 图表汇总下
.echarts-i {

  .table-title {
    font-weight: 700;
    font-size: 30px;
    text-align: center;
    padding: 5px 0 20px 0;
  }

  .echartsb {
    width: 100%;
    display: flex;
    // justify-content: space-between;
    height: 575px;

    .left-echartsb {
      width: 736.5px;
      height: 470px;
    }


    .right-echarte {
      display: flex;
      justify-content: space-around;

      .center-echartsb {
        width: 400px;
        height: 400px;
        margin: 50px 0 0 180px;
      }

      .right-echartsb {
        width: 400px;
        height: 400px;
        margin: 50px 0 0 230px;
      }
    }

  }
}

// 数据汇总表格
.table {

  .table-title {
    font-weight: 700;
    font-size: 30px;
    text-align: center;
    padding: 5px 0 20px 0;
  }

  .scelet {
    float: right;
    padding-right: 20px;
    margin-bottom: 15px;
  }

}
</style>
