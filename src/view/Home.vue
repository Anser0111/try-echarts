<template>
  <div id="home">
    <div id="main" style="width: 600px; height: 600px"></div>
    <button @click="animation">变化</button>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      myChart: {},
      option: {},
      data: [],
      jsonData: [],
      j: 0,
    };
  },
  methods: {
    drawChart() {
      // 基于准备好的dom，初始化echarts实例
      this.myChart = this.$echarts.init(document.getElementById("main"));
      // 指定图表的配置项和数据
      this.option = {
        xAxis: {
          max: "dataMax",
        },
        yAxis: {
          type: "category",
          data: ["A", "B", "C", "D", "E"],
          inverse: true,
          animationDuration: 300,
          animationDurationUpdate: 300,
          max: 4, // only the largest 5 bars will be displayed
        },
        series: [
          {
            realtimeSort: true,
            name: "X",
            type: "bar",
            data: this.data,
            label: {
              show: true,
              position: "right",
              valueAnimation: true,
            },
          },
        ],
        legend: {
          show: true,
        },
        animationDuration: 0, // 初始动画的时长
        animationDurationUpdate: 3000, // 数据更新动画的时长
        animationEasing: "linear",
        animationEasingUpdate: "linear",
      };
      // 使用刚指定的配置项和数据显示图表。
      this.myChart.setOption(this.option);
    },
    run() {
      let data = this.option.series[0].data;
      for (let i = 0; i < data.length; ++i) {
        // if (Math.random() > 0.9) {
        //   data[i] += Math.round(Math.random() * 2000);
        // } else {
        //   data[i] += Math.round(Math.random() * 200);
        // }
        data[i] += this.jsonData[this.j][i];
      }
      this.myChart.setOption(this.option);
    },
    animation() {
      // console.log(this.jsonData[0][0]);
      setTimeout(() => {
        this.run();
        this.j++;
      }, 0);
      setInterval(() => {
        this.run();
        this.j++;
        // console.log(this.j);
        // console.log(this.jsonData[this.j]);
      }, 3000);
    },
  },
  created() {
    const xhr = new XMLHttpRequest();
    // 利用箭头函数没有this！
    xhr.onreadystatechange = () => {
      if (xhr.readyState === 4) {
        if (xhr.status === 200) {
          this.jsonData = xhr.response;
          console.log("成功");
        } else {
          console.log("错误");
        }
      }
    };
    // 设置返回数据的类型。设置成json后，即可直接用。
    xhr.responseType = "json";
    xhr.open("GET", "http://127.0.0.1:8000/hello", true);
    xhr.send();
  },
  mounted() {
    for (let i = 0; i < 5; ++i) {
      this.data.push(Math.round(Math.random() * 2000));
    }
    this.drawChart();
  },
};
</script>

<style>
</style>
