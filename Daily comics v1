<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<title>Daily Comic V2</title>

<style>
body {
  font-family: Arial, sans-serif;
  background: #111;
  color: #eee;
  text-align: center;
  padding: 20px;
}

input, button {
  padding: 10px;
  margin: 5px;
  font-size: 16px;
}

.row {
  display: flex;
  justify-content: center;
  margin: 6px;
}

.cell {
  min-width: 100px;
  padding: 8px;
  margin: 3px;
  border-radius: 5px;
  font-size: 14px;
}

.green { background: #2ecc71; }
.yellow { background: #f1c40f; color:#000; }
.gray { background: #555; }

.guess-name {
  width: 150px;
  font-weight: bold;
  background: #222;
}

#hintBox {
  margin-top: 15px;
  font-style: italic;
  color: #bbb;
}

#shareBtn {
  display: none;
  margin-top: 15px;
}
</style>
</head>

<body>

<h1>🦸 Daily Comic</h1>
<p id="dayNumber"></p>

<input list="characters" id="guessInput" placeholder="Nom du personnage…" />
<datalist id="characters"></datalist>

<br>
<button id="submitBtn">Valider</button>

<div id="board"></div>

<div id="hintBox"></div>

<button id="shareBtn">📋 Partager</button>

<script>
/* ======================
   PERSONNAGES
====================== */
const CHARACTERS = {
  "batman": {
    name: "Batman",
    publisher: "DC",
    city: "Gotham",
    firstAppearance: 1939,
    type: "hero",
    hint: "🦇 Riche orphelin amateur de gadgets"
  },
  "spider-man": {
    name: "Spider-Man",
    publisher: "Marvel",
    city: "New York",
    firstAppearance: 1962,
    type: "hero",
    hint: "🕷️ Grand pouvoir = grandes responsabilités"
  },
  "joker": {
    name: "Joker",
    publisher: "DC",
    city: "Gotham",
    firstAppearance: 1940,
    type: "villain",
    hint: "🤡 Ennemi emblématique de Batman"
  },
  "wolverine": {
    name: "Wolverine",
    publisher: "Marvel",
    city: "Canada",
    firstAppearance: 1974,
    type: "anti-hero",
    hint: "🗡️ Griffes + facteur auto-guérison"
  }
};

/* ======================
   CONFIG
====================== */
const MAX_TRIES = 6;
const STORAGE_KEY = "daily-comic-v2";

/* ======================
   ETAT
====================== */
let state = JSON.parse(localStorage.getItem(STORAGE_KEY)) || {
  tries: [],
  finished: false
};

/* ======================
   INITIALISATION
====================== */
const keys = Object.keys(CHARACTERS);

const startDate = new Date("2026-01-01");
const today = new Date();
const dayIndex = Math.floor((today - startDate) / 86400000);

const answerKey = keys[dayIndex % keys.length];
const answer = CHARACTERS[answerKey];

document.getElementById("dayNumber").textContent =
  "Daily Comic #" + dayIndex;

// autocomplete
const list = document.getElementById("characters");
keys.forEach(k => {
  const opt = document.createElement("option");
  opt.value = CHARACTERS[k].name;
  list.appendChild(opt);
});

render();

/* ======================
   EVENTS
====================== */
document.getElementById("submitBtn").onclick = () => {

  if (state.finished || state.tries.length >= MAX_TRIES) return;

  const input = document.getElementById("guessInput");
  const value = input.value.trim().toLowerCase();

  const guessKey = keys.find(
    k => CHARACTERS[k].name.toLowerCase() === value
  );

  if (!guessKey) {
    alert("Personnage inconnu");
    return;
  }

  const guess = CHARACTERS[guessKey];

  state.tries.push({
    name: guess.name,
    result: compare(guess, answer)
  });

  if (guessKey === answerKey || state.tries.length === MAX_TRIES) {
    state.finished = true;
    document.getElementById("shareBtn").style.display = "inline";
  }

  input.value = "";
  save();
  render();
};

document.getElementById("shareBtn").onclick = share;

/* ======================
   COMPARAISON
====================== */
function compare(guess, answer) {

  return {
    publisher: guess.publisher === answer.publisher ? "green" : "gray",
    city: guess.city === answer.city ? "green" : "gray",
    decade:
      guess.firstAppearance === answer.firstAppearance
        ? "green"
        : Math.abs(guess.firstAppearance - answer.firstAppearance) < 10
        ? "yellow"
        : "gray",
    type: guess.type === answer.type ? "green" : "gray"
  };

}

/* ======================
   RENDER
====================== */
function render() {

  const board = document.getElementById("board");
  board.innerHTML = "";

  state.tries.forEach(t => {

    const row = document.createElement("div");
    row.className = "row";

    const nameCell = document.createElement("div");
    nameCell.className = "cell guess-name";
    nameCell.textContent = t.name;
    row.appendChild(nameCell);

    ["publisher", "city", "decade", "type"].forEach(k => {

      const cell = document.createElement("div");
      cell.className = "cell " + t.result[k];
      cell.textContent = k;
      row.appendChild(cell);

    });

    board.appendChild(row);
  });

  showHint();

  if (state.finished) {
    const msg = document.createElement("p");
    msg.innerHTML = "<strong>Réponse :</strong> " + answer.name;
    board.appendChild(msg);
  }
}

/* ======================
   INDICE PROGRESSIF
====================== */
function showHint() {

  const hintBox = document.getElementById("hintBox");

  if (state.tries.length >= 3 && !state.finished) {
    hintBox.textContent = "Indice : " + answer.hint;
  } else {
    hintBox.textContent = "";
  }
}

/* ======================
   SAUVEGARDE
====================== */
function save() {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(state));
}

/* ======================
   PARTAGE
====================== */
function share() {

  let text = "Daily Comic #" + dayIndex + "\n";

  state.tries.forEach(t => {
    text += ["publisher", "city", "decade", "type"]
      .map(k =>
        t.result[k] === "green" ? "🟩" :
        t.result[k] === "yellow" ? "🟨" : "⬛"
      )
      .join("") + "\n";
  });

  navigator.clipboard.writeText(text);
  alert("Résultat copié !");
}
</script>

</body>
</html>
