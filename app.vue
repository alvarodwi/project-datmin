<template>
  <div class="font-serif w-50% mr-auto ml-4 my-4">
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

    <h3>Hasil Klaster</h3>
    <p>
      Berikut adalah sampel dari klastering tweet yang telah kami lakukan.
      Dibuat 4 klaster berdasarkan hasil analisis sentimen yaitu : positif,
      negatif, netral, dan gabungan atau <i>compound</i>.
    </p>
    <p>
      Suatu tweet dikatakan masuk ke dalam klaster ketika nilai pada klaster
      tersebut lebih tinggi dibanding klaster lainnya. Contohnya, data pertama
      masuk ke klaster netral, data kedua masuk ke klaster negative, dan
      seterusnya.
    </p>
    <table class="w-full text-left text-gray-600">
      <thead class="text-sm text-gray-700 uppercase bg-gray-50">
        <tr>
          <th class="px-4 py-2">positive</th>
          <th class="px-4 py-2">neutral</th>
          <th class="px-4 py-2">negative</th>
          <th class="px-4 py-2">compound</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(r, index) in result" :key="index">
          <td
            class="px-4 py-2"
            :class="r.cluster == 'positive' ? 'font-bold' : ''"
          >
            {{ r.positive }}
          </td>
          <td
            class="px-4 py-2"
            :class="r.cluster == 'neutral' ? 'font-bold' : ''"
          >
            {{ r.neutral }}
          </td>
          <td
            class="px-4 py-2"
            :class="r.cluster == 'negative' ? 'font-bold' : ''"
          >
            {{ r.negative }}
          </td>
          <td
            class="px-4 py-2"
            :class="r.cluster == 'compound' ? 'font-bold' : ''"
          >
            {{ r.compound }}
          </td>
        </tr>
        <tr>
          <td class="px-4 py-2">...</td>
          <td class="px-4 py-2">...</td>
          <td class="px-4 py-2">...</td>
          <td class="px-4 py-2">...</td>
        </tr>
      </tbody>
    </table>

    <h3>Statistik klastering</h3>
    <div class="flex flex-row">
      <div>
        <ClusterLollipopChart :data="cluster" :width="960" :height="500" />
      </div>
      <div>
        <table class="w-60% text-left text-gray-600">
          <thead class="text-sm text-gray-700 uppercase bg-gray-50">
            <tr>
              <th class="px-4 py-2">Cluster</th>
              <th class="px-4 py-2">Total</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(c, index) in cluster" :key="index">
              <td class="px-4 py-2">{{ c.cluster }}</td>
              <td class="px-4 py-2">{{ c.count }}</td>
            </tr>
            <tr class="bg-gray-100 font-bold">
              <td class="px-4 py-2">Total</td>
              <td class="px-4 py-2">{{ totalCluster }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <h3>Wordcloud</h3>
    <div class="absolute">
      <Cloud
        class="mb-4 mx-auto border border-solid border-black"
        :words="words"
        :width="960"
        :height="500"
      />
    </div>
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
const totalCluster = ref(0);

async function resolveCsv() {
  const wordsCsv = await d3.csv("/dataset/words_dict.csv", (d) => {
    return {
      text: d.WORD,
      size: +d.COUNT,
    };
  });

  const resultCsv = await d3.csv("/dataset/result_sample.csv");
  const clusterCsv = await d3.csv("/dataset/cluster.csv", (d) => {
    totalCluster.value += +d.count;
    return {
      cluster: d.cluster,
      count: d.count,
    };
  });

  words.value = wordsCsv
    .sort((word1, word2) => word2.size - word1.size)
    .slice(1, 100);

  result.value = resultCsv;
  cluster.value = clusterCsv;
  console.log(cluster.value);
}

resolveCsv();
</script>
