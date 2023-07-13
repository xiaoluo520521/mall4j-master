<template>
  <div>
  <div style="margin: 10px 0">
  </div>
  <div style="padding: 10px">
    <el-row :gutter="10">
      <!--      <el-col :span="12">-->
      <!--        <el-card style="height:650px;overflow-y:auto;overflow-x:hidden;width: 650px">-->
      <!--          <div id="myChart" :style="{width: '600px', height: '500px'}"></div>-->
      <!--        </el-card>-->
      <!--      </el-col>-->
      <el-col :span="12">
        <el-card style="height:650px;overflow-y:auto;overflow-x:hidden;width: 1300px">
          <div id="myChart2" :style="{width: '1200px', height: '600px'}"></div>
        </el-card>
      </el-col>
    </el-row>

  </div>
  </div>
</template>

<script>
import * as echarts from "echarts";

export default {
  name: "Home",
  data() {
    return {}
  },
  mounted() {
    // this.drawLine();
    this.drawPie();
  },
  methods: {
    drawPie() {
      // 基于准备好的dom，初始化echarts实例
      let myChart = echarts.init(document.getElementById('myChart2'))
      let   option = {
        title: {
          text: '访问用户流量图',
          subtext: '统计数据',
          left: 'left'
        },
        legend: {},
        tooltip: {
          trigger: 'axis',
          showContent: false
        },
        dataset: {
          source: [
            ['product', '2022-10','2022-11', '2022-12', '2023-1', '2023-2', '2023-3', '2023-4'],
            ['萃雅', 56.5, 82.1, 88.7, 70.1, 53.4, 85.1,66.6],
            ['岭南', 51.1, 51.4, 55.1, 53.3, 73.8, 68.7,77,1],
            ['贤德', 40.1, 62.2, 69.5, 36.4, 45.2, 32.5,45.9],
            ['咸嘉', 25.2, 37.1, 41.2, 18, 33.9, 49.1,55.5]
          ]
        },
        xAxis: { type: 'category' },
        yAxis: { gridIndex: 0 },
        grid: { top: '55%',
        },
        series: [
          {
            type: 'line',
            smooth: true,
            seriesLayoutBy: 'row',
            emphasis: { focus: 'series' }
          },
          {
            type: 'line',
            smooth: true,
            seriesLayoutBy: 'row',
            emphasis: { focus: 'series' }
          },
          {
            type: 'line',
            smooth: true,
            seriesLayoutBy: 'row',
            emphasis: { focus: 'series' }
          },
          {
            type: 'line',
            smooth: true,
            seriesLayoutBy: 'row',
            emphasis: { focus: 'series' }
          },
          {
            type: 'pie',
            id: 'pie',
            radius: '30%',
            center: ['50%', '25%'],
            emphasis: {
              focus: 'self'
            },
            label: {
              formatter: '{b}: {@2012} ({d}%)'
            },
            encode: {
              itemName: 'product',
              value: '2012',
              tooltip: '2012'
            }
          }
        ]
      };
      myChart.on('updateAxisPointer', function (event) {
        const xAxisInfo = event.axesInfo[0];
        if (xAxisInfo) {
          const dimension = xAxisInfo.value + 1;
          myChart.setOption({
            series: {
              id: 'pie',
              label: {
                formatter: '{b}: {@[' + dimension + ']} ({d}%)'
              },
              encode: {
                value: dimension,
                tooltip: dimension
              }
            }
          });
        }
      });
      myChart.setOption(option);
    },
    drawLine() {
      // 基于准备好的dom，初始化echarts实例
      let myChart = this.$root.echarts.init(document.getElementById('myChart'))
      let option = {
        title: {
          text: '各地区用户比例统计图',
          subtext: '统计数据',
          left: 'left'
        },
        tooltip: {
          trigger: 'item'
        },
        legend: {
          orient: 'vertical',
          trigger: 'item',
          left: 'center'
        },
        toolbox: {
          show: true,
          feature: {
            mark: {show: true},
            dataView: {show: true, readOnly: false},
            restore: {show: true},
            saveAsImage: {show: true}
          }
        },
        series: [
          {
            name: '用户比例',
            type: 'pie',
            radius: [50, 150],
            center: ['50%', '60%'],
            roseType: 'area',
            itemStyle: {
              borderRadius: 8
            },
            data: []
          }
        ]
      }
      request.get("/user/count").then(res => {
        if (res.code === '0') {
          res.data.forEach(item => {
            option.series[0].data.push({name: item.address, value: item.count})
          })
          // 绘制图表
          myChart.setOption(option);
        }
      })

    }
  }
}
</script>

<style scoped>

</style>
