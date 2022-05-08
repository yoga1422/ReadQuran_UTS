
<template>
  <div class="jumbotron jumbotron-fluid">
    <div class="container text-center">
      <h1 class="mt-5 display-4 fw-bolder">{{ surah.name_simple }}</h1>
      <p class="lead mb-5 text-black">{{ artiSurah.name }}</p>
    </div>
  </div>
  <div class="mb-5">
    <div v-for="(ayat, index) in arrayAyat" :key="index" class="card">
      <div class="card-body text-end">
        <h5 class="card-title ayat d-inline text-center">( {{ index + 1 }} )</h5>
        <h5 class="card-title nomorAyat d-inline display-5 align-center">{{ ayat.text_uthmani }}</h5>
        <br />
        <div class="mt-3">
          <p class="card-text mt-3 d-inline"><strong> Artinya : </strong></p>
          <p class="card-text mt-3 artiAyat d-inline" v-html="arrayTerjemahAyat[index].text"></p>
          <audio controls class="audio1">
          <source :src="AudioAyat.audio_url" type="audio">
          </audio>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { ref } from "@vue/reactivity";
export default {
  data() {
    return {
      surah: "",
      artiSurah: "",
      arrayAyat: ref([]),
      arrayTerjemahAyat: ref([]),
      listSurah: ref([]),
      AudioAyat: "",
    };
  },
  mounted() {
    this.getAyat();
    this.getSurah();
    this.getListSurah();
    this.getTerjemahAyat();
    this.getAudioAyat()
  },
  methods: {
    getSurah() {
      axios
        .get(`https://api.quran.com/api/v4/chapters/${this.$route.params.id}?language=id`)
        .then((response) => {
          this.surah = response.data.chapter;
          console.log(this.surah);
          this.artiSurah = this.surah.translated_name;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    getAyat() {
      axios
        .get(`https://api.quran.com/api/v4/quran/verses/uthmani?chapter_number=${this.$route.params.id}`)
        .then((response) => {
          this.arrayAyat = response.data.verses;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    getListSurah() {
      axios
        .get(`https://api.quran.com/api/v4/chapters?language=id`)
        .then((response) => {
          this.listSurah = response.data.chapters;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    getTerjemahAyat() {
      axios
        .get(`https://api.quran.com/api/v4/quran/translations/33?chapter_number=${this.$route.params.id}`)
        .then((response) => {
          this.arrayTerjemahAyat = response.data.translations;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    getAudioAyat(){
      axios
      .get('https://api.quran.com/api/v4/chapter_recitations/1?language=id')
      .then((response)=> {
        this.AudioAyat = response.data.audio_files;
      })
      .catch((error) => {
        console.log(error);
      });

    },
  },
};
</script>