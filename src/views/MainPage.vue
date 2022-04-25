<template>
  <div class="container">
    <div class="box">
      <ul>
        <li v-for="(japanese_word, index) in japanese_words" :key="index">
          <span class="perfect-circle">
            <span class="circle">{{ japanese_word }} {{index}}</span>
          </span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data(){
    return {
      japanese_words: 'おこそとのほもよろんえけせてねへめえれゑうくすつぬふむゆるをいきしちにひみいりゐあかさたなはまやらわ'.split('').reverse()
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