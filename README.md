# Emma Azariadi
Hello, this is me. My favorite character is Kermit the frog. Welcome to our blog. I like to dance... and by “dance,” I mean I wiggle my arms like a flailing inflatable tube man while whispering “it’s not easy being green” to myself in the mirror. My moves are so smooth that last week a pigeon stopped to watch and then slowly walked away in shame.

On this blog you’ll find all my hottest content: reviews of swamps I have never visited, a 47-part series on lily pads (part 12 is a real rollercoaster), and my daily motivational quote, which is always just “ribbit” with the same energy as a LinkedIn influencer. Smash that follow button like I smash my hips into the corner of the kitchen table while practicing the Macarena.

Fun fact: I once tried to moonwalk at a wedding and ended up just walking backwards while apologizing. The bride called it “brave.” My mom called it “a cry for help.” I call it art.

So grab a juice box, sit back, and let’s hop into this journey together. Remember: if you can’t dance like nobody’s watching, at least dance like a frog is watching. 🐸💃 (Kermit sips tea. That’s none of my business.)

<button onclick="window.open('https://www.youtube.com/watch?v=lA3sieD4MvE&t=683s')" style="font-size:20px; padding:12px 24px; cursor:pointer;">
  This is me! Yay heheheha!
</button>
<button onclick="window.open('https://www.youtube.com/watch?v=dQw4w9WgXcQ')">Click me!</button>

<img width="391" height="397" alt="image" src="https://github.com/user-attachments/assets/233042ca-57d4-450a-90ae-a7e008923ba5" />
<img width="391" height="397" alt="image" src="https://github.com/user-attachments/assets/bfaa0186-d112-4516-920e-79ee1bc51c70" />

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Frog Tic Tac Toe</title>
<style>
  :root {
    --pond: #1d4a38;
    --water: #e6f2dc;
    --pad: #9fd36b;
    --pad-dark: #6faa3c;
    --ink: #12261c;
    --gold: #f3c94b;
  }
  * { box-sizing: border-box; }
  body {
    margin: 0;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 16px;
    padding: 20px;
    background: var(--water);
    color: var(--ink);
    font-family: Georgia, "Times New Roman", serif;
    text-align: center;
  }
  h1 { margin: 0; font-size: clamp(28px, 7vw, 44px); color: var(--pond); }
  #status { font-size: 22px; min-height: 30px; margin: 0; }
  #board {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 10px;
    width: min(92vw, 360px);
    padding: 10px;
    background: var(--pond);
    border-radius: 18px;
  }
  .cell {
    aspect-ratio: 1;
    font-size: clamp(36px, 13vw, 64px);
    background: var(--pad);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    box-shadow: inset 0 -6px 0 var(--pad-dark);
    transition: transform 0.1s;
  }
  .cell:hover:not(:disabled) { transform: scale(1.04); }
  .cell:disabled { cursor: default; }
  .cell:focus-visible, #reset:focus-visible { outline: 4px solid var(--gold); outline-offset: 2px; }
  .cell.win { background: var(--gold); box-shadow: inset 0 -6px 0 #d1a728; }
  #scores { display: flex; gap: 24px; font-size: 20px; }
  #reset {
    font-family: inherit;
    font-size: 20px;
    padding: 10px 26px;
    color: white;
    background: var(--pond);
    border: none;
    border-radius: 999px;
    cursor: pointer;
  }
  #reset:hover { background: #2b6a50; }
  @media (prefers-reduced-motion: reduce) { .cell { transition: none; } }
</style>
</head>
<body>
  <h1>Frog Tic Tac Toe 🐸</h1>
  <div id="scores">
    <span>🐸 Frog: <b id="scoreX">0</b></span>
    <span>🪰 Fly: <b id="scoreO">0</b></span>
    <span>Draws: <b id="scoreD">0</b></span>
  </div>
  <p id="status" aria-live="polite"></p>
  <div id="board"></div>
  <button id="reset">New game</button>

<script>
  const MARKS = { X: "🐸", O: "🪰" };
  const LINES = [
    [0,1,2],[3,4,5],[6,7,8],
    [0,3,6],[1,4,7],[2,5,8],
    [0,4,8],[2,4,6]
  ];
  const boardEl = document.getElementById("board");
  const statusEl = document.getElementById("status");
  const scores = { X: 0, O: 0, D: 0 };
  let cells = [];
  let board, turn, over;

  // Build the 9 cells once
  for (let i = 0; i < 9; i++) {
    const b = document.createElement("button");
    b.className = "cell";
    b.setAttribute("aria-label", "Square " + (i + 1));
    b.addEventListener("click", () => play(i));
    boardEl.appendChild(b);
    cells.push(b);
  }

  function newGame() {
    board = Array(9).fill("");
    turn = "X";
    over = false;
    cells.forEach(c => {
      c.textContent = "";
      c.disabled = false;
      c.classList.remove("win");
    });
    statusEl.textContent = "Frog's turn " + MARKS.X;
  }

  function play(i) {
    if (over || board[i]) return;
    board[i] = turn;
    cells[i].textContent = MARKS[turn];
    cells[i].disabled = true;

    const line = LINES.find(l => l.every(k => board[k] === turn));
    if (line) {
      over = true;
      line.forEach(k => cells[k].classList.add("win"));
      scores[turn]++;
      statusEl.textContent = (turn === "X" ? "Frog" : "Fly") + " wins! " + MARKS[turn];
    } else if (board.every(Boolean)) {
      over = true;
      scores.D++;
      statusEl.textContent = "It's a draw. Ribbit.";
    } else {
      turn = turn === "X" ? "O" : "X";
      statusEl.textContent = (turn === "X" ? "Frog" : "Fly") + "'s turn " + MARKS[turn];
    }

    if (over) {
      cells.forEach(c => c.disabled = true);
      document.getElementById("scoreX").textContent = scores.X;
      document.getElementById("scoreO").textContent = scores.O;
      document.getElementById("scoreD").textContent = scores.D;
    }
  }

  document.getElementById("reset").addEventListener("click", newGame);
  newGame();
</script>
</body>
</html>


