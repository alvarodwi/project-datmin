<script setup lang="ts">
import * as d3 from "d3";

interface Cluster {
  cluster: string;
  count: number;
}

const props = defineProps({
  data: Array<Cluster>,
  width: Number,
  height: Number,
});

watch(
  () => props.data,
  (first, _second) => {
    draw(first);
  }
);

function draw(data: Array<Cluster>) {
  var margin = { top: 10, right: 30, bottom: 90, left: 40 },
    width = 460 - margin.left - margin.right,
    height = 500 - margin.top - margin.bottom;

  // append the svg object to the body of the page
  var svg = d3
    .select("#lollipopChart")
    .append("svg")
    .attr("width", width + margin.left + margin.right)
    .attr("height", height + margin.top + margin.bottom)
    .append("g")
    .attr("transform", "translate(" + margin.left + "," + margin.top + ")");

  // X axis
  var x = d3
    .scaleBand()
    .range([0, width])
    .domain(
      data.map(function (d: Cluster) {
        return d.cluster;
      })
    )
    .padding(1);
  svg
    .append("g")
    .attr("transform", "translate(0," + height + ")")
    .call(d3.axisBottom(x))
    .selectAll("text")
    .attr("transform", "translate(-10,0)rotate(-45)")
    .style("text-anchor", "end");

  // Add Y axis
  var y = d3.scaleLinear().domain([0, 2700]).range([height, 0]);
  svg.append("g").call(d3.axisLeft(y));

  // Lines
  svg
    .selectAll("myline")
    .data(data)
    .enter()
    .append("line")
    .attr("x1", function (d: Cluster) {
      return x(d.cluster);
    })
    .attr("x2", function (d: Cluster) {
      return x(d.cluster);
    })
    .attr("y1", function (d: Cluster) {
      return y(d.count);
    })
    .attr("y2", y(0))
    .attr("stroke", "grey");

  // Circles
  svg
    .selectAll("mycircle")
    .data(data)
    .enter()
    .append("circle")
    .attr("cx", function (d: Cluster) {
      return x(d.cluster);
    })
    .attr("cy", function (d: Cluster) {
      return y(d.count);
    })
    .attr("r", "4")
    .style("fill", "#69b3a2")
    .attr("stroke", "black");
}
</script>

<template>
  <div id="lollipopChart" ref="lollipopChart"></div>
</template>
