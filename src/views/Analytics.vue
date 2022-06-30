<template>
  <div class="analytic">
    <div class="analytic__schedule" ref="chartdiv">
      <Amcharts5/>

    </div>
  </div>
</template>


<script>
  import * as am5 from '@amcharts/amcharts5';
  import * as am5xy from '@amcharts/amcharts5/xy';
  import am5themes_Animated from '@amcharts/amcharts5/themes/Animated';


  export default  {
    mounted() {
      let root = am5.Root.new(this.$refs.chartdiv);

      root.setThemes([
        am5themes_Animated.new(root)
      ]);

      //let data = [
      //  { date: new Date(2012, 0, 1).getTime(), value: 8 },
      //{ date: new Date(2012, 0, 2).getTime(), value: 10 },
      //{ date: new Date(2012, 0, 3).getTime(), value: 12 },
      //{ date: new Date(2012, 0, 7).getTime(), value: 7 },
      //{ date: new Date(2012, 0, 8).getTime(), value: 9 },
      //{ date: new Date(2012, 0, 9).getTime(), value: 13 },
      //{ date: new Date(2012, 0, 13).getTime(), value: 22 },
      //{ date: new Date(2012, 0, 14).getTime(), value: 20 }
      //];
      let data = [
          {date: new Date(2020, 6, 1).getTime(),visits:213},
          {date: new Date(2020, 6, 2).getTime(),visits:249},
          {date: new Date(2020, 6, 3).getTime(),visits:179},
          {date: new Date(2020, 6, 4).getTime(),visits:170},
          {date: new Date(2020, 6, 5).getTime(),visits:184},
          {date: new Date(2020, 6, 6).getTime(),visits:202},
          {date: new Date(2020, 6, 7).getTime(),visits:198},
          {date: new Date(2020, 6, 8).getTime(),visits:168},
          {date: new Date(2020, 6, 9).getTime(),visits:176},
          {date: new Date(2020, 6, 10).getTime(),visits:171},
          {date: new Date(2020, 6, 11).getTime(),visits:190},
          {date: new Date(2020, 6, 12).getTime(),visits:154},
          {date: new Date(2020, 6, 13).getTime(),visits:246},
          {date: new Date(2020, 6, 14).getTime(),visits:250},
          {date: new Date(2020, 6, 15).getTime(),visits:227},
          {date: new Date(2020, 6, 16).getTime(),visits:140},
          {date: new Date(2020, 6, 17).getTime(),visits:170},
          {date: new Date(2020, 6, 18).getTime(),visits:125},
          {date: new Date(2020, 6, 19).getTime(),visits:106},
          {date: new Date(2020, 6, 20).getTime(),visits:207},
          {date: new Date(2020, 6, 21).getTime(),visits:222},
          {date: new Date(2020, 6, 22).getTime(),visits:198},
          {date: new Date(2020, 6, 23).getTime(),visits:204},
          {date: new Date(2020, 6, 24).getTime(),visits:213},
          {date: new Date(2020, 6, 25).getTime(),visits:145},
          {date: new Date(2020, 6, 26).getTime(),visits:166},
          {date: new Date(2020, 6, 27).getTime(),visits:163},
          {date: new Date(2020, 6, 28).getTime(),visits:135},
          {date: new Date(2020, 6, 29).getTime(),visits:45}
      ]



// Create chart
// https://www.amcharts.com/docs/v5/charts/xy-chart/
      let chart = root.container.children.push(
          am5xy.XYChart.new(root, {
            panX: true,
            panY: true,
            wheelX: "panX",
            wheelY: "zoomX"
          })
      );

      let easing = am5.ease.linear;

// Create axes
// https://www.amcharts.com/docs/v5/charts/xy-chart/axes/
      let xAxis = chart.xAxes.push(
          am5xy.GaplessDateAxis.new(root, {
            maxDeviation: 0.1,
            groupData: false,
            baseInterval: {
              timeUnit: "day",
              count: 1
            },
            renderer: am5xy.AxisRendererX.new(root, {
              minGridDistance: 50
            }),
            tooltip: am5.Tooltip.new(root, {})
          })
      );

      let yAxis = chart.yAxes.push(
          am5xy.ValueAxis.new(root, {
            maxDeviation: 0.1,
            renderer: am5xy.AxisRendererY.new(root, {})
          })
      );

// Add series
// https://www.amcharts.com/docs/v5/charts/xy-chart/series/
      let series = chart.series.push(
          am5xy.LineSeries.new(root, {
            minBulletDistance: 10,
            xAxis: xAxis,
            yAxis: yAxis,
            valueYField: "visits",
            valueXField: "date",
            tooltip: am5.Tooltip.new(root, {
              pointerOrientation: "horizontal",
              labelText: "{valueY}"
            })
          })
      );

      series.data.setAll(data);

      series.bullets.push(function () {
        return am5.Bullet.new(root, {
          sprite: am5.Circle.new(root, {
            radius: 5,
            fill: series.get("fill"),
            stroke: root.interfaceColors.get("background"),
            strokeWidth: 2
          })
        });
      });

// Add cursor
// https://www.amcharts.com/docs/v5/charts/xy-chart/cursor/
      let cursor = chart.set("cursor", am5xy.XYCursor.new(root, {
        xAxis: xAxis
      }));
      cursor.lineY.set("visible", false);

// add scrollbar
      chart.set("scrollbarX", am5.Scrollbar.new(root, {
        orientation: "horizontal"
      }));

// Make stuff animate on load
// https://www.amcharts.com/docs/v5/concepts/animations/
      series.appear(1000, 100);
      chart.appear(1000, 100);
    },
  }

</script>

<style lang="scss" scoped>
.analytic {
  .analytic__schedule {
    width:800px;
    height: 500px;
  }
}


</style>
