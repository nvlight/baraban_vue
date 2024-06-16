<script setup>

import {onMounted, ref} from "vue";

const staticZeroDeg = ref(27);

const resultOptions = {
  one_attempt: '1 попытка',
  points_for_game: 'Баллы для игры',
  five_hundred_points_pyaterochka: '500 баллов (Пятерочка)',
  one_month_litres: '1 месяц (Литрес)',
  sticker_pack_telegram_from_clear: 'Стикер пак для телеграмм от Clear',
  one_month_yandex_plus: '1 месяц Яндекс плюс',
  half_attempt: '0.5 попытки',
};
// const optionsDegs = {
//   one_attempt: 27,
//   points_for_game: 334,
//   five_hundred_points_pyaterochka: 77,
//   one_month_litres: 282,
//   sticker_pack_telegram_from_clear: 129,
//   one_month_yandex_plus: 181,
//   half_attempt: 232,
// };
const optionsDegs = {
  one_attempt: 27 - staticZeroDeg.value,
  points_for_game: 334 - staticZeroDeg.value,
  five_hundred_points_pyaterochka: 77 - staticZeroDeg.value,
  one_month_litres: 282 - staticZeroDeg.value,
  sticker_pack_telegram_from_clear: 129 - staticZeroDeg.value,
  one_month_yandex_plus: 181 - staticZeroDeg.value,
  half_attempt: 232 - staticZeroDeg.value,
};
const resultsKeyValue = [
  'one_attempt',
  'points_for_game',
  'five_hundred_points_pyaterochka',
  'one_month_litres',
  'sticker_pack_telegram_from_clear',
  'one_month_yandex_plus',
  'half_attempt',
];
const probabilities = [0.1, 0.2, 0.1, 0.2, 0.2, 0.1, 0.1];
const chosenNumber = ref(null);

const deg = ref(0);
const prize = ref('');
const showPrize = ref(false);

const weightedRandomChoice = (probabilities) => {
  const randNum = Math.random();
  let totalProb = 0;

  for (let num = 0; num < probabilities.length; num++) {
    totalProb += probabilities[num];
    if (randNum < totalProb) {
      chosenNumber.value = num;
      break;
    }
  }
};

const start = () => {
  showPrize.value = false;

  weightedRandomChoice(probabilities);

  let resKey = resultsKeyValue[chosenNumber.value];
  deg.value = staticZeroDeg.value + optionsDegs[resKey];

  setTimeout(()=> {

    prize.value = resultOptions[resKey];

    showPrize.value = true;
  },3000);
}

onMounted( () => {
  //prize.value = resultOptions.five_hundred_points_pyaterochka;
  //showPrize.value = true;
  deg.value = staticZeroDeg.value;
});

</script>

<template>
  <div class="wrapper">
    <div id="imgs">
      <img
          id="other_place"
          src="./assets/img/other2.jpg" alt="">
      <img
          id="baraban"
          :style="{ transform: 'rotate(' + deg + 'deg)' }"
          src="./assets/img/baraban_nice_4.png"
          alt=""
      >
      <img id="arrow"
           src="./assets/img/arrow_nice_3.png"
           alt=""
      >
    </div>
    <button @click="start" class="btn-run">запустить</button>
    <div v-if="showPrize" id="result">Приз: <br><span>{{ prize }}</span></div>
  </div>
</template>

<style scoped>
.wrapper {
  width: 1280px;
  margin: 0 auto;
  background-color: #010c22;
  position: relative;

  #arrow{
    position: absolute;
    top: 72px;
    left: 390px;
    z-index: 3;
  }

  #result{
    position: absolute;
    top: 466px;
    left: 788px;
    font-size: 35px;
    text-align: center;
    width: 388px;
    span{
      font-style: italic;
      font-size: 29px;
    }
  }

  .btn-run{
    position: absolute;
    top: 285px;
    left: 796px;
    font-size: 43px;
    text-transform: uppercase;
    border-radius: 90px;
    padding: 47px 59px;
    border: none;
    font-weight: bold;
    box-shadow: 0px 0px 20px 7px #fc973d;
    background-image: linear-gradient(372deg, #e42e41, #f8c035);
    cursor: pointer;
  }

  #imgs{
    position: relative;
    #baraban{
      position: absolute;
      top: 74px;
      left: 142px;

      transition: 3s;
      transform: rotate(0deg);
    }
  }

  h1{
    font-size: 125px;
    margin-top: 0;
  }

  .tmp_ul {
    display: flex;
    flex-wrap: wrap;

    > li {
      margin: 0 10px;
    }
  }
}

.section_columns {
  display: flex;
  justify-content: space-around;
}
</style>
