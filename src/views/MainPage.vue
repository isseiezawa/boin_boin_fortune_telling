<template>
  <div class="container">
    <div class="box">
      <random-words-box :japanese_words="japanese_words" :random_number="random_number" />
      <select-box :value="selected_number" @input="selected_number = $event" :options="select_options" />
      <div class="d-grid gap-2 col-6 mx-auto text-center">
        <button @click="start_or_stop ? loopProcessing(100) : slowLoop()" class="btn btn-primary">{{ startOrStopButton }}</button>
      </div>
    </div>
    <div class="d-grid gap-2 col-6 mx-auto text-center">
      <h2>{{result_word}}</h2>
    </div>
  </div>
</template>

<script>
import RandomWordsBox from '../components/RandomWordsBox.vue'
import SelectBox from '../components/SelectBox.vue'

export default {
  components: {
    RandomWordsBox,
    SelectBox
  },
  data(){
    return {
      japanese_words: 'おこそとのほもよろんえけせてねへめえれゑうくすつぬふむゆるをいきしちにひみいりゐあかさたなはまやらわ'.split('').reverse(),
      selected_number: '2',
      select_options: [
          {text: '2個', value: '2'},
          {text: '3個', value: '3'},
          {text: '4個', value: '4'},
          {text: '5個', value: '5'},
          {text: '6個', value: '6'},
        ],
      random_number: [],
      result_word: "",
      start_or_stop: true,
      change_loop: null,
      loop_bgm: new Audio(require('../assets/bgm/loop_bgm.mp3'))
    }
  },
  created(){
    this.loop_bgm.loop = true;
    speechSynthesis.getVoices(); // 声の配列をあらかじめ取得
  },
  computed: {
    startOrStopButton() {
      return this.start_or_stop ? 'スタート' : 'ストップ';
    }
  },
  methods: {
    randomPickupNumber() {
      var arr = [];
      var numArr = [];
      for(var i = 0; i < this.japanese_words.length; i++){
        arr[i] = i;
      }
      for(var j = 0, len = arr.length; j < this.selected_number; j++, len--) {
        var rndNum = Math.floor(Math.random()*len);
        numArr.push(arr[rndNum]);
        arr[rndNum] = arr[len-1];
      }
      console.log(numArr)
      this.random_number = numArr
      this.resultWordBox()
    },
    resultWordBox() {
      this.result_word = ""
      if(this.random_number.length) {
        for(var i = 0; i < this.selected_number; i++) {
          this.result_word += this.japanese_words[this.random_number[i]]
        }
      }
    },
    loopProcessing(time) {
      if(!this.change_loop) {
        this.start_or_stop = false;
        this.loop_bgm.play();
        this.change_loop = setInterval(this.randomPickupNumber, time) //  指定した時間ごとに処理を実行する内容をdataに格納
      }
    },
    slowLoop() {
      clearInterval(this.change_loop);
      this.loop_bgm.playbackRate = 0.5; // ループ時のBGMをゆっくりに
      this.loopProcessing(1000);
      setTimeout(this.stopLoop, 3.0*1000) // 3秒後にloop処理を止める
    },
    stopLoop() {
      clearInterval(this.change_loop);
      this.change_loop = null;
      this.start_or_stop = true;
      this.loop_bgm.pause();
      this.loop_bgm.playbackRate = 1.0;
      this.getVoice();
    },
    getVoice() {
      const uttr = new SpeechSynthesisUtterance();
      const voice = speechSynthesis.getVoices(); // 利用可能な音声のリストを格納
      uttr.voice = voice[49]; // 声指定
      uttr.volume = 0.5; //音量
      uttr.rate   = 0.2; //読み上げ速度
      uttr.pitch  = 0; //音程
      uttr.text = this.result_word // 読み上げる内容
      speechSynthesis.cancel(); // 発声キューの削除
      speechSynthesis.speak(uttr); // 読み上げ
    }
  }
}
</script>