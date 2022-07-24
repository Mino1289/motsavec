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
    <label>Nombre de lettre : </label>
    <button class="btn-counter" @click="increment"> + </button>
    <span id="number"> {{ number }} </span>
    <button class="btn-counter" @click="decrement"> - </button>
    <input id="comptage" type="checkbox" name="comptage" checked> <label for="comptage">Taille<p>(la taille est compté dans la recherche)</p></label>
  </div>
  <div id="select" class="select">
    <div v-if="number != 0" class="select">
      <label>Position des lettres : </label>
      <input v-for="n in number" :key="n" class="letter" type="text" maxlength="1">

    </div>
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
      number: 0,
    };
  },
  methods: {
    main() {
      var starts = document.getElementById("startby").value.toLowerCase();
      var ends = document.getElementById("endby").value.toLowerCase();
      var inside = document.getElementById("include").value.toLowerCase();
      var notinside = document.getElementById("notinclude").value.toLowerCase();
      var sizes = this.number;
      var suites = document.getElementById("suites").value.toLowerCase();
      var letters = document.getElementsByClassName("letter");
      var lataille = document.getElementById("comptage").checked;
      
      var i = 0;
      let arr = [];
      while (letters.item(i) != null) {
        if (letters.item(i).value.toLowerCase() !== " ") {
          arr.push(letters.item(i).value.toLowerCase());
        } else {
          arr.push("");
        }
        i++;
      }

      var url = "https://motsavecapi.herokuapp.com/";
      var params = {
        start: starts == undefined ? "" : starts,
        end: ends == undefined ? "" : ends,
        in: inside == undefined ? "" : inside,
        nin: notinside == undefined ? "" : notinside,
        size: sizes == undefined || !lataille ? "" : sizes,
        suite: suites == undefined ? "" : suites,
        let: i == 0 ? "" : "[\""+arr.join("\",\"")+"\"]",
      };
      Object.keys(params).forEach(function(key) {
        if (params[key] === "" || params[key] === undefined || params[key] === null || params[key] === 0) {
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
          console.error(error);
        });
      
    },
    reset() {
      document.getElementById("startby").value = "";
      document.getElementById("endby").value = "";
      document.getElementById("include").value = "";
      document.getElementById("notinclude").value = "";
      document.getElementById("number").innerText = "0";
      document.getElementById("suites").value = "";
      document.getElementById("resp").innerText = "";
      document.getElementById("counter").innerText = "";
      this.number = 0;
    },
    increment() {
      this.number++;
      this.update();
    },
    decrement() {
      this.number-1 < 0 ? 0 : this.number--;
      this.update();
    },
    update() {
      var select = document.getElementById("select");
      if (select != null) {
      select.onkeyup = function(e) {
        var target = e.srcElement;
        var maxLength = parseInt(target.attributes["maxlength"].value, 10);
        var myLength = target.value.length;
        if (myLength >= maxLength) {
          var next = target;
          /*eslint no-cond-assign: "off"*/
          while (next = next.nextElementSibling) {
            if (next == null)
              break;
            if (next.tagName.toLowerCase() === "input") {
              next.focus();
              break;
            }
          }
        }
      }}
    }
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

.letter {
  width: 15px;
  height: 15px;
  text-align: center;
  margin-left: 5px;
  margin-right: 5px;
}

.btn-counter {
  width: 20px;
  height: 21px;
  text-align: center;
}

#number {
  margin-left: 5px;
  margin-right: 5px;
}

#comptage {
  margin-left: 15px;
}
</style>
