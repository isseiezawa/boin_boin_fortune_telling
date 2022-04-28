<template>
  <div class="container">
    <div class="box">
      <RandomWordsBox :japanese_words="japanese_words" :random_number="random_number" />
      <div class="form-group col-6 mx-auto text-center my-3">
        <label for="boin-quantity">ボイン数選択</label>
        <select v-model="selected_number" class="form-control text-center" id="boin-quantity">
          <option value="2">2個</option>
          <option value="3">3個</option>
          <option value="4">4個</option>
          <option value="5">5個</option>
          <option value="6">6個</option>
        </select>
      </div>
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

export default {
  components: {
    RandomWordsBox
  },
  data(){
    return {
      japanese_words: 'おこそとのほもよろんえけせてねへめえれゑうくすつぬふむゆるをいきしちにひみいりゐあかさたなはまやらわ'.split('').reverse(),
      selected_number: 2,
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
      // 0から50を入れる処理
      for(var i = 0; i < 50; i++){
        arr[i] = i;
      }
      for(var j = 0, len = arr.length; j < this.selected_number; j++, len--) {
        // 3.代入された要素数-1の値は下記のrandomでは選ばれない
        var rndNum = Math.floor(Math.random()*len);
        // 1.arr[n]番目の数をpush
        numArr.push(arr[rndNum]);
        // 2.arrの要素数-1の数字がarr[rudNum]に代入
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