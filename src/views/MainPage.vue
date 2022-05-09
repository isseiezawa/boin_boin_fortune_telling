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
        this.change_loop = setInterval(this.randomPickupNumber, time)
      }
    },
    slowLoop() {
      clearInterval(this.change_loop);
      this.change_loop = null;
      this.loop_bgm.playbackRate = 0.5;
      this.loopProcessing(1000);
      // 3秒後にloop処理を止める
      setTimeout(this.stopLoop, 3.0*1000)
    },
    stopLoop() {
      this.start_or_stop = true;
      this.loop_bgm.pause();
      this.loop_bgm.playbackRate = 1.0;
      clearInterval(this.change_loop);
      this.change_loop = null;
    }
  }
}
</script>