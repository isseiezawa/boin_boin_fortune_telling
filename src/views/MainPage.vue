<template>
  <div class="container">
    <div class="box">
      <ul>
        <li v-for="(japanese_word, index) in japanese_words" :key="index">
          <span class="perfect-circle">
            <span class="circle">{{ japanese_word }}</span>
          </span>
          <div v-if="random_words.includes(index)">ボイン</div>
        </li>
      </ul>
      <!-- <div>{{randomPickupNumber()}}</div> -->
      <select v-model="selected_number">
        <option>2</option>
        <option>3</option>
        <option>4</option>
        <option>5</option>
        <option>6</option>
      </select>
      <div class="d-grid gap-2 col-6 mx-auto text-center">
        <button @click="start_or_stop ? loopProcessing(100) : slowLoop()" class="btn btn-primary">{{ startOrStopButton }}</button>
      </div>
    </div>
    <button @click="randomPickupNumber()">ボイン</button>
  </div>
</template>

<script>
export default {
  data(){
    return {
      japanese_words: 'おこそとのほもよろんえけせてねへめえれゑうくすつぬふむゆるをいきしちにひみいりゐあかさたなはまやらわ'.split('').reverse(),
      selected_number: 2,
      random_number: [],
      start_or_stop: true,
      change_loop: null,
  computed: {
    startOrStopButton() {
      return this.start_or_stop ? 'スタート' : 'ストップ';
    }
  },
  methods: {
    randomPickupNumber: function() {
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
      return this.random_number = numArr
    },
    loopProcessing(time) {
      if(!this.change_loop) {
        this.start_or_stop = false;
        this.change_loop = setInterval(this.randomPickupNumber, time)
      }
    },
    slowLoop() {
      clearInterval(this.change_loop);
      this.change_loop = null;
      this.loopProcessing(1000);
      // 3秒後にloop処理を止める
      setTimeout(this.stopLoop, 3.0*1000)
    },
    stopLoop() {
      this.start_or_stop = true;
      clearInterval(this.change_loop);
      this.change_loop = null;
    }
  }
}
</script>

<style scoped>
ul{
  /* flexboxで等間隔に並べる*/
  display: flex;
  flex-wrap: wrap;
  border: solid 1px;
  text-align: center;
  padding: 0;
  margin: 10px;
}

li{
  /* 枠線をつけて1列に5つ並べる */
  border: solid 1px;
  border-top-color: rgb(255, 249, 224);
  border-left-color: rgb(242, 176, 78);
  border-right-color: rgb(242, 176, 78);
  border-bottom-color: rgb(120, 86, 36);
  border-radius: 100%;
  width: 10%;
  /* 正方形にするのに必要な部分 */
  height: auto;
  position: relative;
  list-style: none;
  font-size: 1%;
  background-color: rgb(255, 249, 224);
}
li:before {
  content: "";
  display: block;
  padding-top: 100%; /* ここを100％にすることで正方形になる */
}
li span.perfect-circle{
  /* 正方形にするのに必要な部分 */
  display: block;
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  /* 中央寄せ */
  text-align: center;
  padding: 2px;
  box-sizing: border-box;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  -o-box-sizing: border-box;
  -ms-box-sizing: border-box;
}
/* 高さの中央揃えに必要な部分 */
li span.perfect-circle:before{
  content: "";
  height: 100%;
  vertical-align: middle;
  width: 0px;
  display: inline-block;
}


.circle {
  border: solid 3px rgb(249, 206, 213);
  border-radius: 100%;
  background-color: rgb(237, 127, 145);
}

</style>