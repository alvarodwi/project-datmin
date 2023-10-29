<template>
  <div class="font-serif w-60% mx-auto my-4">
    <div id="header">
      <h1>Klasterisasi Tweet mengenai Indihome</h1>
      <p>Data Mining Universitas Padjadjaran 2023</p>

      <div id="identity">
        <p>Oleh :</p>
        <p>Ariq Hakim Ruswadi - 140810200001</p>
        <p>Rommel Malik Kusnadi - 140810200002</p>
        <p>Rifqy Kurnia Sudarman - 140810200039</p>
        <p>Alvaro Dwi Oktaviano - 140810200041</p>
      </div>
    </div>

    <h2>Overview dari Project</h2>
    <p>Lorem ipsum</p>

    <h2>Tahapan Pengerjaan</h2>
    <h3>Pengumpulan Data</h3>
    <p>Lorem</p>

    <h3>Pemrosesan Data</h3>
    <p>Lorem</p>

    <h3>Metode Klasterisasi</h3>
    <p>Lorem</p>

    <h2>Hasil Project</h2>

    <h3>Hasil Klaster</h3>
    <p>Berikut adalah sampel dari klustering tweet yang telah kami lakukan</p>
    <table>
      <thead>
        <tr>
          <th>positive</th>
          <th>neutral</th>
          <th>negative</th>
          <th>compound</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(r, index) in result" :key="index">
          <td>{{ r.positive }}</td>
          <td>{{ r.neutral }}</td>
          <td>{{ r.negative }}</td>
          <td>{{ r.compound }}</td>
        </tr>
      </tbody>
    </table>

    <h3>Statistik Klustering</h3>
    <ClusterLollipopChart :data="cluster" :width="960" :height="500" />
    <table>
      <thead>
        <tr>
          <th>Cluster</th>
          <th>Total</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(c, index) in cluster" :key="index">
          <td>{{ c.cluster }}</td>
          <td>{{ c.count }}</td>
        </tr>
      </tbody>
    </table>

    <h3>Wordcloud</h3>

    <Cloud
      class="border border-solid border-black"
      :words="words"
      :width="960"
      :height="500"
    />
  </div>
</template>

<script setup>
import * as d3 from "d3";
import "@unocss/reset/sanitize/sanitize.css";
import "@unocss/reset/sanitize/assets.css";

useHead({
  title: "Klasterisasi Tweet mengenai Indihome - Data Mining 2023",
});

const words = ref([]);
const result = ref([]);
const cluster = ref([]);

async function resolveCsv() {
  const wordsCsv = await d3.csv("/dataset/words_dict.csv", (d) => {
    return {
      text: d.WORD,
      size: +d.COUNT,
    };
  });

  const resultCsv = await d3.csv("/dataset/result_sample.csv");
  const clusterCsv = await d3.csv("/dataset/cluster.csv");

  words.value = wordsCsv
    .sort((word1, word2) => word2.size - word1.size)
    .slice(1, 100);

  result.value = resultCsv;
  cluster.value = clusterCsv;
  console.log(cluster.value);
}

resolveCsv();
</script>
