<template>
  <h1><u><strong>Trouver un mot qui: </strong></u></h1>

  <div class="select">
    <label>Commence par : </label>
    <input class="input" id="startby" type="text" value="" />
  </div>
  <div class="select">
    <label>Termine par : </label>
    <input class="input" id="endby" type="text" value="" />
  </div>
  <div class="select">
    <label>Contient ces lettres : </label>
    <input class="input" id="include" type="text" value="" />
  </div>
  <div class="select">
    <label>Contient ces lettres dans l'ordre : </label>
    <input class="input" id="suites" type="text" value="" />
  </div>
  <div class="select">
    <label>Ne contient pas ces lettres : </label>
    <input class="input" id="notinclude" type="text" value="" />
  </div>
  <div class="select">
    <label>A un nombre de lettre : </label>
    <select id="selector">
      <option class="options" value="">Choissisez un nombre de lettre</option>
      <option class="options" value="1">1</option>
      <option class="options" value="2">2</option>
      <option class="options" value="3">3</option>
      <option class="options" value="4">4</option>
      <option class="options" value="5">5</option>
      <option class="options" value="6">6</option>
      <option class="options" value="7">7</option>
      <option class="options" value="8">8</option>
      <option class="options" value="9">9</option>
      <option class="options" value="10">10</option>
      <option class="options" value="11">11</option>
      <option class="options" value="12">12</option>
      <option class="options" value="13">13</option>
      <option class="options" value="14">14</option>
      <option class="options" value="15">15</option>
      <option class="options" value="16">16</option>
      <option class="options" value="17">17</option>
      <option class="options" value="18">18</option>
      <option class="options" value="19">19</option>
      <option class="options" value="20">20</option>
      <option class="options" value="21">21</option>
      <option class="options" value="22">22</option>
      <option class="options" value="23">23</option>
      <option class="options" value="24">24</option>
      <option class="options" value="25">25</option>
    </select>
  </div>

  <button class="btn" type="button" @click="main">Chercher</button><br />
  <button class="btn" type="button" @click="reset">Reset</button>
  <h4 class="resp" id="counter"></h4>

  <p class="resp" id="resp">{{ response }}</p>
</template>

<script>
const axios = require('axios').default;

export default {
  data() {
    return {
      response: undefined,
    };
  },
  methods: {
    main() {
      var starts = document.getElementById("startby").value.toLowerCase();
      var ends = document.getElementById("endby").value.toLowerCase();
      var inside = document.getElementById("include").value.toLowerCase();
      var notinside = document.getElementById("notinclude").value.toLowerCase();
      var sizes = document.getElementById("selector").value.toLowerCase();
      var suites = document.getElementById("suites").value.toLowerCase();
      var url = "https://motsavecapi.herokuapp.com/";
      var params = {
        start: starts == undefined ? "" : starts,
        end: ends == undefined ? "" : ends,
        in: inside == undefined ? "" : inside,
        nin: notinside == undefined ? "" : notinside,
        size: sizes == undefined ? "" : sizes,
        suite: suites == undefined ? "" : suites,
      };
      Object.keys(params).forEach(function(key) {
        if (params[key] === "") {
          delete params[key];
        }
      });
      axios
        .get(url, {
          params: params,
        })
        .then((response) => {
          var resp =  document.getElementById("resp");
          resp.innerText = response.data.join(", ");
          var counter = document.getElementById("counter");
          counter.innerText = `${response.data.length} / 336531 = ${Math.round((response.data.length / 336531) * 1000000)/10000}%`;
        })
        .catch((error) => {
          console.log(error);
        });
      
    },
    reset() {
      document.getElementById("startby").value = "";
      document.getElementById("endby").value = "";
      document.getElementById("include").value = "";
      document.getElementById("notinclude").value = "";
      document.getElementById("selector").value = "";
      document.getElementById("suites").value = "";
      document.getElementById("resp").innerText = "";
      document.getElementById("counter").innerText = "";
    },
  },
};
</script>

<style scoped>
* {
  font-family: "Segoe", Tahoma, Verdana, sans-serif;
  padding: 0;
  margin: 0;
}
#container {
  padding-top: 50px;
  text-align: center;
}
.title {
  padding: 5px;
}

.btn {
  margin: 5px;
  width: 300px;
}

.select {
  padding: 5px;
}

.resp {
  padding: 5px;
}
</style>
