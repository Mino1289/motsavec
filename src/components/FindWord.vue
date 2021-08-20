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
import lesmots from "../assets/data.json";

/**
 * Count the number of occurence of s in the string
 * @param {String} s
 * @returns
 */
String.prototype.count = function (s) {
  var result = 0;
  for (let i = 0; i < this.length; i++) {
    if (this[i] == s) result++;
  }
  return result;
};

/**
 *
 * @param {String[]} L la liste de lettre à tester
 * @param {String} m le mot à tester
 * @returns
 */
const fil = (L, m) => {
  var f = [];
  for (let i = 0; i < L.length; i++) {
    if (
      m.includes(L[i]) &&
      m.count(L[i]) >= L.filter((x) => x == L[i]).length
    ) {
      f.push(L[i]);
    }
  }
  if (f.length == L.length) return true;
  else return false;
};

/**
 *
 * @param {String[]} L la liste de lettre à tester
 * @param {String} m le mot à tester
 * @returns
 */
const nofil = (L, m) => {
  var f = [];
  for (let i = 0; i < L.length; i++) {
    if (m.includes(L[i])) {
      f.push(L[i]);
    }
  }
  if (f.length == 0) return true;
  else return false;
};

/**
 * @param {String[]} L Liste de lettre qui ne doivent pas etre dans le mots.
 * @param {String[]} M Les mots à filtrer
 */
const nocontain = (L, M) => {
  var E = [];
  M.forEach((mot) => {
    if (nofil(L, mot)) {
      E.push(mot);
    }
  });
  return E;
};

/**
 * @param {String[]} L Les lettres
 * @param {String[]} M Les mots à filtrer
 */
const contain = (L, M) => {
  var E = [];
  M.forEach((mot) => {
    if (fil(L, mot)) {
      E.push(mot);
    }
  });
  return E;
};

/**
 * @param {String} s le mot commence par s
 * @param {String[]} Mots Liste des mots a filtrer
 */
const start = (s, M) => {
  var E = [];
  M.forEach((mot) => {
    if (mot.startsWith(s)) E.push(mot);
  });
  return E;
};

/**
 * @param {String} s le mot fini par s
 * @param {String[]} Mots Liste des mots a filtrer
 */
const end = (s, M) => {
  var E = [];
  M.forEach((mot) => {
    if (mot.endsWith(s)) E.push(mot);
  });
  return E;
};
/**
 *
 * @param {Number} n
 * @param {String[]} M
 */
const sized = (n, M) => {
  var E = [];
  M.forEach((mot) => {
    if (mot.length === n) E.push(mot);
  });
  return E;
};

/**
 * @param {String} s suite de caractère dans la liste de mot
 * @param {String[]} M liste de mot
 */
const suite = (s, M) => {
  var E = [];
  M.forEach((mot) => {
    if (mot.includes(s)) E.push(mot);
  });
  return E;
};

export default {
  data() {
    return {
      response: undefined,
    };
  },
  methods: {
    main() {
      var starts = document.getElementById("startby").value;
      var ends = document.getElementById("endby").value;
      var inside = document.getElementById("include").value;
      var notinside = document.getElementById("notinclude").value;
      var sizes = document.getElementById("selector").value;
      var suites = document.getElementById("suites").value;

      var E = lesmots;
      if (starts) {
        E = start(starts, E || lesmots);
      }
      if (ends) {
        E = end(ends, E || lesmots);
      }
      if (inside) {
        E = contain(inside.split(""), E || lesmots);
      }
      if (notinside) {
        E = nocontain(notinside.split(""), E || lesmots);
      }
      if (suites) {
        E = suite(suites, E || lesmots);
      }
      if (sizes) {
        E = sized(parseInt(sizes), E || lesmots);
      }
      var resp = document.getElementById("resp");
      resp.innerText = E.join(", ");
      var counter = document.getElementById("counter");
      counter.innerText = `${E.length} / ${lesmots.length} = ${
        Math.round((E.length / lesmots.length) * 1000000) / 10000
      }%`;
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
