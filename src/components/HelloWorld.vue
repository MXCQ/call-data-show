<template>
  <body>
    <!-- 头部 -->
    <div class="top">
      <img src="../assets/1.png" alt class="img1" />
      <span>工业互联网重要资源测绘与安全分析平台</span>
      <img src="../assets/2.png" alt class="img2" />
    </div>
    <div class="viewport">
      <div class="column left">
        <!--工控设备-->
        <div class="bd ec1">
          <div class="bd-header">工控设备</div>
          <div class="pie"></div>
          <a
            href="javascript:;"
            class=""
            v-for="item in leftTopSelect"
            :key="item.value"
            @click="letEc1($event, item.value)"
          >
            <i class="icon-dot" style="color: #006cff">{{ item.label }}</i></a
          >
        </div>
        <!--中国工控设备状态-->
        <div class="bd ec2">
          <div class="bar-a"></div>
          <a
            href="javascript:;"
            class=""
            v-for="item in leftCenterSelect"
            :key="item.value"
            @click="letEc2($event, item.value)"
          >
            <i class="icon-dot" style="color: #006cff">{{ item.label }}</i></a
          >
        </div>
        <!--工控地理分布图-->
        <div class="bd ec3">
          <div class="bar-b"></div>
        </div>
      </div>
      <div class="column middle">
        <div class="m-one">
          <!--健康-->
          <div class="m-one-1 bd2"></div>
          <!--工控个数-->
          <div class="m-one-2 bd2"></div>
          <!--工控种类-->
          <div class="m-one-3 bd2"></div>
        </div>
        <!--地图-->
        <div class="m-two"></div>
        <!--事件/告警数据趋势-->
        <div class="m-three">
          <div class="ele"></div>
        </div>
      </div>
      <div class="column right">
        <!--工控漏洞-->
        <div class="bd3 r-one">
          <div class="bd-header">工控漏洞</div>
          <div class="rightTopData"></div>
          <a href="javascript:;" class="active">
            <i class="icon-dot" style="color: #006cff"></i>漏洞类型分布</a
          >
          <a href="javascript:;">
            <i class="icon-dot" style="color: #006cff"></i>设备协议分布</a
          >
          <a href="javascript:;" @click="letEc1($event, 'treatyLT')">
            <i class="icon-dot" style="color: #006cff"></i>厂商分布</a
          >
        </div>
        <!--工控设备漏洞状态-->
        <div class="bd3 r-two">
          <div class="rightCenterData"></div>
        </div>
        <!--安全预警日志-->
        <div class="bd3 r-three">
          <div class="bd-header">安全预警日志</div>
          <ul id="log">
            <li v-for="item in logList" :key="item">
              <span></span>
              {{ item.content }}
            </li>
          </ul>
        </div>
      </div>
    </div>
  </body>
</template>

<script>
import echarts from "echarts";
import $ from "jquery";
export default {
  name: "HelloWorld",
  data() {
    return {
      // 安全预警日志数据
      logList: [
        { content: "发现工控漏洞信息23条" },
        { content: " 新发现僵尸设备16个" },
        { content: "  新发现漏洞设备8个" },
        { content: " 新发现僵尸设备98个" },
        { content: " 新发现漏洞设备12个" },
        { content: " 新发现漏洞设备23个" },
      ],
      curLogList: [],
      // 交互切换数据（两组）
      leftTopSelect: [
        {
          value: "typeLT",
          label: "设备类型分布",
        },
        {
          value: "treatyLT",
          label: "设备协议分布",
        },
        {
          value: "distributionLT",
          label: "厂商分布",
        },
      ],
      leftCenterSelect: [
        {
          value: "typeLC",
          label: "设备类型分布",
        },
        {
          value: "statisticsLC",
          label: "行业设备分布",
        },
        {
          value: "distributionLC",
          label: "企业分布",
        },
      ],
      ec1Data: {
        // 工控设备切换数据
        typeLT: [
          { name: "SDS", value: 977 },
          { name: "PLC", value: 815 },
          { name: "SCADA", value: 251 },
          { name: "DCS_C", value: 86 },
          { name: "RTU", value: 60 },
          { name: "RPD", value: 23 },
          { name: "Communications Adapter", value: 22 },
          { name: "CPIE", value: 8 },
          { name: "HMI", value: 7 },
        ],
        distributionLT: [
          { name: "moxa", value: 1027 },
          { name: "advantech", value: 247 },
          { name: "siemens", value: 241 },
          { name: "schneider", value: 234 },
          { name: "omron", value: 122 },
          { name: "allen-bradley", value: 111 },
          { name: "abb", value: 110 },
          { name: "rockwell", value: 73 },
          { name: "honeywell", value: 31 },
          // {name: "其他", value: 128}
        ],
        treatyLT: [
          { name: "telnet", value: 557 },
          { name: "moxanport", value: 466 },
          { name: "http", value: 309 },
          { name: "modbus", value: 264 },
          { name: "s7", value: 218 },
          { name: "ethernetip", value: 185 },
          { name: "https", value: 151 },
          { name: "snmp", value: 49 },
          { name: "ftp", value: 46 },
          // {name: "其他", value: 91}
        ],
      },
      ec2Data: {
        typeLC: {
          xData: [
            "SDS",
            "PLC",
            "DAM",
            "SCADA",
            "DCS_C",
            "HMI",
            "RTU",
            "RPD",
            "Communications Adapter",
          ],
          yData: [988, 868, 452, 251, 86, 73, 70, 23, 22],
        },
        statisticsLC: {
          xData: [
            "能源",
            "交通",
            "汽车",
            "家电",
            "医疗",
            "银行",
            "自动化",
            "快递",
            "服务",
          ],
          yData: [1383, 1366, 1166, 1037, 986, 969, 810, 428, 334],
        },
        distributionLC: {
          xData: [
            "机械",
            "交通",
            "汽车",
            "家电",
            "医疗",
            "银行",
            "餐饮",
            "快递",
            "服务",
          ],
          yData: [163, 166, 156, 137, 126, 119, 110, 108, 104],
        },
      },
      ec3Data: {
        title: "工控蜜罐地理分布图",
        xData: [
          "新北市",
          "高雄市",
          "台北市",
          "台南市",
          "云林县",
          "台中市",
          "苗栗县",
          "桃园市",
          "上海",
          "阜阳",
        ],
        yData: [62, 39, 33, 21, 17, 14, 10, 9, 8, 8],
      },
      // 健康数值
      centerTopLeft: {
        value: 77,
      },
    };
  },
  mounted() {
    // 工控设备
    this.ec1(this.ec1Data.typeLT);
    this.ec2(this.ec2Data.typeLC);
    this.ec3();
    this.cTL();
    this.midBot();
    this.rTD();
    this.rCD();
  },
  methods: {
    // 安全预警日志
    pushLog() {
      setInterval(() => {}, 5000);
    },

    // 工控设备切换
    letEc1(e, data) {
      // 样式
      $(e.target)
        .addClass("active")
        .siblings()
        .removeClass("active");
      var currData = this.ec1Data[data];
      this.ec1(currData);
    },
    // 工控设备
    ec1(data) {
      var option = {
        tooltip: {
          trigger: "item",
          formatter: "{a} <br/>{b} : {c} ({d}%)",
        },
        series: [
          {
            name: "",
            type: "pie",
            minAngle: 5,
            avoidLabelOverlap: true,
            radius: ["30%", "50%"],
            center: ["50%", "50%"],
            data: data,
          },
        ],
      };
      var myChart = echarts.init($(".pie")[0]);
      myChart.setOption(option);
    },
    // 中国工控设备状态切换
    letEc2(e, data) {
      // 样式
      $(e.target)
        .addClass("active")
        .siblings()
        .removeClass("active");
      var currData = this.ec2Data[data];
      this.ec2(currData);
    },
    // 中国工控设备状态
    ec2(data) {
      var option = {
        title: {
          text: "中国工控设备状态",
          left: "center",
          textStyle: {
            color: "#ccc",
          },
        },
        xAxis: {
          type: "category",
          data: data.xData,
          axisLabel: {
            interval: 0,
            rotate: 40,
            color: "#ccc",
            fontSize: 10,
          },
        },
        yAxis: {
          type: "value",
          axisLabel: {
            color: "#ccc",
            fontSize: 10,
          },
        },
        series: [
          {
            data: data.yData,
            type: "bar",
            color: {
              colorStops: [
                {
                  offset: 0,
                  color: "skyblue", // 0% 处的颜色
                },
                {
                  offset: 1,
                  color: "blue", // 100% 处的颜色
                },
              ],
            },
          },
        ],
      };
      var myChart = echarts.init($(".bar-a")[0]);
      myChart.setOption(option);
    },
    // 工控蜜罐地理分布图
    ec3() {
      var option = {
        title: {
          text: this.ec3Data.title,
          left: "center",
          textStyle: {
            color: "#ccc",
          },
        },
        grid: {
          left: "3%",
          right: "4%",
          bottom: "3%",
          containLabel: true,
        },
        xAxis: {
          type: "value",
          boundaryGap: [0, 0.01],
          axisLabel: {
            color: "#ccc",
            fontSize: 10,
          },
        },
        yAxis: {
          type: "category",
          data: this.ec3Data.xData,
          axisLabel: {
            interval: 0,
            rotate: 40,
            color: "#ccc",
            fontSize: 10,
          },
        },
        series: [
          {
            name: "2011年",
            type: "bar",
            data: this.ec3Data.yData,
            color: "skyblue",
          },
        ],
      };
      var myChart = echarts.init($(".bar-b")[0]);
      myChart.setOption(option);
    },
    // 健康数值
    cTL() {
      var option = {
        tooltip: {
          formatter: "{a} <br/>{b} : {c}%",
        },
        toolbox: {
          feature: {
            restore: {},
            saveAsImage: {},
          },
        },
        series: [
          {
            type: "gauge",
            radius: "100%",
            detail: { show: false },
            data: [{ value: this.centerTopLeft.value }],
            axisLabel: {
              // 刻度标签。
              color: "#fff",
              fontSize: 12,
              formatter: "{value}",
            },
          },
        ],
      };
      var myChart = echarts.init($(".m-one-1")[0]);
      myChart.setOption(option);
    },
    // 事件/告警数量趋势图数据
    midBot() {
      var option = {
        xAxis: {
          type: "category",
          boundaryGap: false,
          data: [
            "2019 12-24",
            "2019 12-25",
            "2019 12-26",
            "2019 12-27",
            "2019 12-28",
            "2019 12-29",
            "2019 12-30",
          ],
        },
        yAxis: {
          type: "value",
        },
        series: [
          {
            data: [150, 232, 201, 154, 190, 330, 410],
            type: "line",
            areaStyle: {
              color: "red",
            },
          },
          {
            data: [120, 132, 101, 134, 90, 230, 210],
            type: "line",
            areaStyle: {
              color: "#333",
            },
          },
        ],
      };
      var myChart = echarts.init($(".ele")[0]);
      myChart.setOption(option);
    },
    // 工控漏洞数据
    rTD() {
      var option = {
        tooltip: {
          trigger: "item",
          formatter: "{a} <br/>{b} : {c} ({d}%)",
        },
        series: [
          {
            type: "pie",
            radius: "65%",
            center: ["50%", "50%"],
            selectedMode: "single",
            data: [
              { name: "未分类", value: 207 },
              { name: "缓冲区错误", value: 73 },
              { name: "信息泄露", value: 70 },
              { name: "缓冲区边界操作限制不当", value: 69 },
              { name: "输入验证不当", value: 60 },
              { name: "跨站脚本", value: 42 },
              { name: "路径遍历", value: 29 },
              { name: "跨站请求伪造", value: 20 },
              { name: "授权问题", value: 20 },
            ],
            emphasis: {
              itemStyle: {
                shadowBlur: 10,
                shadowOffsetX: 0,
                shadowColor: "rgba(0, 0, 0, 0.5)",
              },
            },
          },
        ],
      };
      var myChart = echarts.init($(".rightTopData")[0]);
      myChart.setOption(option);
    },
    // 中国工控设备漏洞状况数据
    rCD() {
      var option = {
        title: {
          text: "中国工控设备漏洞状况数据",
          left: "center",
          textStyle: {
            color: "#ccc",
          },
        },
        xAxis: {
          type: "category",
          data: ["危急", "高危", "中危"],
          axisLabel: {
            interval: 0,
            rotate: 40,
            color: "#ccc",
            fontSize: 10,
          },
        },
        yAxis: {
          type: "value",
          axisLabel: {
            color: "#ccc",
            fontSize: 10,
          },
        },
        series: [
          {
            data: [26, 15, 4],
            type: "bar",
            barWidth: 20, //柱图宽度
            color: {
              colorStops: [
                {
                  offset: 0,
                  color: "skyblue", // 0% 处的颜色
                },
                {
                  offset: 1,
                  color: "blue", // 100% 处的颜色
                },
              ],
            },
          },
        ],
      };
      var myChart = echarts.init($(".rightCenterData")[0]);
      myChart.setOption(option);
    },
  },
};
</script>

<style scoped lang="less">
* {
  margin: 0;
  padding: 0;
}
h4,
h3,
ul {
  margin: 0;
  padding: 0;
  font-weight: normal;
}
ul {
  list-style: none;
}
a {
  text-decoration: none;
}
// ----------------头部-----------------
.top {
  height: 40px;
  width: 100%;
  background-color: #02234d;
  .img1 {
    position: absolute;
    top: 10px;
    height: 40px;
    width: 80px;
  }
  span {
    font-size: 20px;
    color: white;
    line-height: 40px;
    padding-left: 90px;
  }
  .img2 {
    position: absolute;
    top: 8px;
    right: 10px;
    height: 40px;
    width: 80px;
  }
}

.bd-header {
  margin-top: 5px;
  color: #ccc;
  text-align: center;
}

.viewport {
  max-width: 1920px;
  min-width: 1024px;
  margin: 0 auto;
  min-height: 780px;
  display: flex;

  .column {
    flex: 2;
  }
  .column:nth-of-type(2) {
    flex: 4;
    margin: 1.333rem 0.833rem 0;
  }

  // 左边
  .left {
    width: 100%;
    padding: 20px;
    .bd {
      height: 260px;
      border: 1px solid blue;
      border-radius: 5px;
    }
    /* 工控设备 */
    .ec1 {
      overflow: hidden;
      box-sizing: border-box;
      .pie {
        margin-top: 20px;
        height: 175px;
      }
      a {
        font-size: 14px;
        padding: 3px;
      }
    }
    .ec1 a.active,
    .ec2 a.active {
      color: #a3c6f2;
      background-color: rgba(10, 67, 188, 0.2);
    }

    /* 中国工控设备状态 */
    .ec2 {
      margin: 20px 0 20px 0;
      .bar-a {
        height: 220px;
      }
    }

    // 左边下面表格
    .ec3 {
      .bar-b {
        height: 220px;
      }
    }
  }

  // 中间
  .middle {
    // 中间上边盒子
    .m-one {
      height: 160px;
      width: 100%;
      display: flex;
      .bd2 {
        border: solid #043568 2px;
        border-radius: 5px;
      }
      .m-one-1 {
        flex: 2;
      }
      .m-one-2 {
        flex: 2;
        margin: 0 20px 0 20px;
      }
      .m-one-3 {
        flex: 3;
      }
    }
    .m-two {
      height: 455px;
      width: 100%;
    }
    .m-three {
      height: 200px;
      border: solid #043568 2px;
      border-radius: 5px;
      .ele {
        // width: 300px;
        height: 200px;
      }
    }
  }

  // ------------右边--------------
  .right {
    flex: 1;
    margin-left: 35px;
    margin-top: 20px;

    .bd3 {
      height: 260px;
      width: 340px;
      border: solid #043568 2px;
      border-radius: 5px;
    }
    .rightTopData {
      height: 205px;
    }
    .rightCenterData {
      height: 250px;
    }

    .r-two {
      margin: 20px 0 20px 0;
    }
    .r-three {
      #log {
        li {
          height: 38px;
          line-height: 38px;
          color: rgb(140,140,140);
          font-size: 14px;
        }
      }
    }
  }
}
</style>
