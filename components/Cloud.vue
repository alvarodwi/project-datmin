<script setup lang="ts">
import * as cloud from "d3-cloud";
import * as d3 from "d3";

interface Words {
  text: String;
  size: Number;
}

const props = defineProps({
  words: Array<Words>,
  width: Number,
  height: Number,
});

watch(
  () => props.words,
  (first, second) => {
    createCanvas(first);
  }
);

function createCanvas(words: Array<Words>) {
  const layout = cloud()
    .size([props.width, props.height])
    .words(words)
    .padding(5)
    .rotate(() => (Math.random() * 6 - 3) * 30)
    .fontSize((d) => d.size / 3)
    .on("end", draw);

  layout.start();
}

function draw(words: Array<Words>) {
  d3.select("#wordCloud")
    .append("svg")
    .attr("width", props.width)
    .attr("height", props.height)
    .append("g")
    .attr("transform", `translate(${props.width / 2},${props.height / 2})`)
    .selectAll("text")
    .data(words)
    .enter()
    .append("text")
    .style("fill", d3.scaleOrdinal(d3.schemeCategory10))
    .style("font-size", function (d) {
      return d.size + "px";
    })
    .style("font-family", "Impact")
    .attr("text-anchor", "middle")
    .attr("transform", function (d) {
      return "translate(" + [d.x, d.y] + ")rotate(" + d.rotate + ")";
    })
    .text(function (d) {
      return d.text;
    });
}
</script>

<template>
  <div id="wordCloud" ref="wordCloud"></div>
</template>
