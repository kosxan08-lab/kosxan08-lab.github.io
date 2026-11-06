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
  a { color: var(--pond); align-self: flex-start; }
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
  #scores { display: flex; gap: 24px; font-size: 20px; flex-wrap: wrap; justify-content: center; }
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
  <a href="index.html">← Back to blog</a>
  <h1>Frog Tic Tac Toe 🐸</h1>
  <div id="scores">
    <span>🪰 Me: <b id="scoreMe">0</b></span>
    <span>🐸 Frog: <b id="scoreFrog">0</b></span>
    <span>Draws: <b id="scoreD">0</b></span>
  </div>
  <p id="status" aria-live="polite"></p>
  <div id="board"></div>
  <button id="reset">New game</button>

<script>
  // "me" is the human (the fly), "frog" is the bot that plays random moves
  const MARKS = { me: "🪰", frog: "🐸" };
  const NAMES = { me: "Me", frog: "Frog" };
  const LINES = [
    [0,1,2],[3,4,5],[6,7,8],
    [0,3,6],[1,4,7],[2,5,8],
    [0,4,8],[2,4,6]
  ];
  const boardEl = document.getElementById("board");
  const statusEl = document.getElementById("status");
  const scores = { me: 0, frog: 0, D: 0 };
  let cells = [];
  let board, turn, over, botTimer;

  for (let i = 0; i < 9; i++) {
    const b = document.createElement("button");
    b.className = "cell";
    b.setAttribute("aria-label", "Square " + (i + 1));
    b.addEventListener("click", () => {
      if (turn === "me") play(i);
    });
    boardEl.appendChild(b);
    cells.push(b);
  }

  function newGame() {
    clearTimeout(botTimer);
    board = Array(9).fill("");
    turn = "me";
    over = false;
    cells.forEach(c => {
      c.textContent = "";
      c.disabled = false;
      c.classList.remove("win");
    });
    statusEl.textContent = "Your turn " + MARKS.me;
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
      statusEl.textContent = turn === "me" ? "You win! 🪰🎉" : "Frog wins! 🐸";
    } else if (board.every(Boolean)) {
      over = true;
      scores.D++;
      statusEl.textContent = "It's a draw. Ribbit.";
    } else if (turn === "me") {
      turn = "frog";
      statusEl.textContent = "Frog is thinking... 🐸";
      botTimer = setTimeout(frogMove, 600);
    } else {
      turn = "me";
      statusEl.textContent = "Your turn " + MARKS.me;
    }

    if (over) {
      cells.forEach(c => c.disabled = true);
      document.getElementById("scoreMe").textContent = scores.me;
      document.getElementById("scoreFrog").textContent = scores.frog;
      document.getElementById("scoreD").textContent = scores.D;
    }
  }

  // The frog picks a random empty square
  function frogMove() {
    const empty = [];
    board.forEach((v, i) => { if (!v) empty.push(i); });
    const pick = empty[Math.floor(Math.random() * empty.length)];
    play(pick);
  }

  document.getElementById("reset").addEventListener("click", newGame);
  newGame();
</script>
</body>
</html>




<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Blog with a friendly girl</title>
<style>
  body { font-family: Georgia, serif; text-align: center; background: #e6f2dc; color: #12261c; padding: 40px 16px; }
</style>
</head>
<body>
<h1>Welcome to my blog 🐸</h1>
<p>The girl in the corner waves when you click her.</p>

<!-- GIRL WIDGET START: copy everything from here down to the END comment and paste it before </body> in your index.html -->
<style>
  #girl-box {
    position: fixed; right: 8px; bottom: 0;      /* change "right" to "left" to move her to the other corner */
    width: min(220px, 42vw); aspect-ratio: 220 / 290;
    z-index: 9999; pointer-events: none;
  }
  #girl-box canvas { display: block; width: 100%; height: 100%; }
  #girl-bubble {
    position: absolute; left: 50%; top: 2%; transform: translateX(-50%);
    background: #fff; color: #12261c; padding: 6px 12px; border-radius: 14px;
    font: 15px Georgia, serif; white-space: nowrap;
    box-shadow: 0 2px 8px rgba(0,0,0,.25); opacity: 0; transition: opacity .3s;
  }
</style>
<div id="girl-box"><div id="girl-bubble"></div></div>
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script>
(function () {
  const box = document.getElementById("girl-box");
  const bubble = document.getElementById("girl-bubble");

  const scene = new THREE.Scene();
  const camera = new THREE.PerspectiveCamera(30, 220 / 290, 0.1, 50);
  camera.position.set(0, 1.9, 9);
  camera.lookAt(0, 1.9, 0);

  const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
  renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
  box.appendChild(renderer.domElement);
  function resize() {
    const w = box.clientWidth, h = box.clientHeight;
    renderer.setSize(w, h, false);
    camera.aspect = w / h;
    camera.updateProjectionMatrix();
  }
  window.addEventListener("resize", resize);
  resize();

  scene.add(new THREE.AmbientLight(0xffffff, 0.85));
  const sun = new THREE.DirectionalLight(0xffffff, 0.8);
  sun.position.set(2, 4, 5);
  scene.add(sun);

  // ----- Materials and helper -----
  const mat = (c, r = 0.6) => new THREE.MeshStandardMaterial({ color: c, roughness: r });
  const skin = mat(0xffd9b8, 0.7), hairM = mat(0xf7d65a, 0.55), dressM = mat(0xf06aa8),
        white = mat(0xffffff), shoeM = mat(0xd6335b), dark = mat(0x234a8c, 0.3),
        pinkM = mat(0xff9db8), lipM = mat(0xb23a48);
  function add(parent, geo, m, x, y, z) {
    const o = new THREE.Mesh(geo, m);
    o.position.set(x, y, z);
    parent.add(o);
    return o;
  }

  // ----- Body -----
  const girl = new THREE.Group();
  scene.add(girl);

  [-1, 1].forEach(s => {
    add(girl, new THREE.CylinderGeometry(0.09, 0.09, 0.8, 12), skin, s * 0.2, 0.5, 0);
    add(girl, new THREE.SphereGeometry(0.14, 14, 10), shoeM, s * 0.2, 0.1, 0.07).scale.set(1, 0.6, 1.5);
  });
  add(girl, new THREE.CylinderGeometry(0.34, 0.72, 1.1, 28), dressM, 0, 1.45, 0);
  add(girl, new THREE.TorusGeometry(0.43, 0.03, 8, 28), white, 0, 1.75, 0).rotation.x = Math.PI / 2;
  add(girl, new THREE.TorusGeometry(0.72, 0.03, 8, 32), white, 0, 0.91, 0).rotation.x = Math.PI / 2;
  add(girl, new THREE.CylinderGeometry(0.09, 0.09, 0.22, 12), skin, 0, 2.07, 0);

  function makeArm(side) {
    const shoulder = new THREE.Group();
    shoulder.position.set(side * 0.38, 1.93, 0);
    girl.add(shoulder);
    add(shoulder, new THREE.SphereGeometry(0.11, 12, 10), dressM, 0, 0, 0);
    add(shoulder, new THREE.CylinderGeometry(0.075, 0.07, 0.55, 12), skin, 0, -0.28, 0);
    const elbow = new THREE.Group();
    elbow.position.y = -0.56;
    shoulder.add(elbow);
    add(elbow, new THREE.CylinderGeometry(0.07, 0.06, 0.5, 12), skin, 0, -0.25, 0);
    add(elbow, new THREE.SphereGeometry(0.1, 12, 10), skin, 0, -0.54, 0);
    return { shoulder, elbow };
  }
  const armL = makeArm(-1);
  const armR = makeArm(1);   // the arm on the right side of the screen is the waving one

  // ----- Head -----
  const head = new THREE.Group();
  head.position.y = 2.6;
  girl.add(head);
  add(head, new THREE.SphereGeometry(0.5, 28, 22), skin, 0, 0, 0);

  // Blonde hair: fringe cap on top, fuller hair behind, two pigtails
  add(head, new THREE.SphereGeometry(0.54, 28, 18, 0, Math.PI * 2, 0, Math.PI * 0.38), hairM, 0, 0.02, -0.01).rotation.x = 0.2;
  add(head, new THREE.SphereGeometry(0.55, 24, 18), hairM, 0, 0.02, -0.13).scale.set(1, 0.85, 1);

  const tails = [];
  [-1, 1].forEach(s => {
    const g = new THREE.Group();
    g.position.set(s * 0.5, 0.02, -0.06);
    head.add(g);
    add(g, new THREE.SphereGeometry(0.085, 10, 8), pinkM, s * 0.04, 0, 0);
    add(g, new THREE.SphereGeometry(0.2, 14, 12), hairM, s * 0.09, -0.36, 0).scale.set(0.6, 1.8, 0.6);
    g.userData.s = s;
    tails.push(g);
  });

  // Face
  const eyes = [];
  [-1, 1].forEach(s => {
    const e = add(head, new THREE.SphereGeometry(0.075, 14, 12), dark, s * 0.17, -0.03, 0.44);
    e.scale.set(1, 1.25, 0.6);
    eyes.push(e);
    add(head, new THREE.SphereGeometry(0.025, 8, 8), white, s * 0.17 + 0.02, 0.0, 0.49);
    add(head, new THREE.SphereGeometry(0.07, 12, 10), pinkM, s * 0.3, -0.13, 0.37).scale.set(1, 0.65, 0.4);
  });
  const smile = new THREE.TorusGeometry(0.09, 0.017, 8, 18, Math.PI);
  smile.rotateZ(Math.PI);
  add(head, smile, lipM, 0, -0.09, 0.47);

  // Soft shadow on the ground
  const shadow = new THREE.Mesh(
    new THREE.CircleGeometry(0.85, 24).rotateX(-Math.PI / 2),
    new THREE.MeshBasicMaterial({ color: 0x000000, transparent: true, opacity: 0.2 })
  );
  shadow.position.y = 0.005;
  scene.add(shadow);

  // ----- Speech bubble -----
  let bubbleTimer;
  function say(text, ms) {
    bubble.textContent = text;
    bubble.style.opacity = 1;
    clearTimeout(bubbleTimer);
    bubbleTimer = setTimeout(() => { bubble.style.opacity = 0; }, ms);
  }
  say("Click me! 👋", 4500);

  // ----- Wave on click -----
  const DUR = 2.6;
  let waveT = -1;
  const ray = new THREE.Raycaster();
  const pt = new THREE.Vector2();
  window.addEventListener("pointerdown", e => {
    const r = renderer.domElement.getBoundingClientRect();
    if (e.clientX < r.left || e.clientX > r.right || e.clientY < r.top || e.clientY > r.bottom) return;
    pt.set(((e.clientX - r.left) / r.width) * 2 - 1, -((e.clientY - r.top) / r.height) * 2 + 1);
    ray.setFromCamera(pt, camera);
    if (ray.intersectObject(girl, true).length && waveT < 0) {
      waveT = 0;
      say("Hi there! 👋", DUR * 1000);
    }
  });

  // ----- Animation loop -----
  const clock = new THREE.Clock();
  let nextBlink = 2;
  function frame() {
    requestAnimationFrame(frame);
    const dt = Math.min(clock.getDelta(), 0.05);
    const t = clock.elapsedTime;

    // Idle: breathing, gentle sway, head tilt
    girl.position.y = Math.sin(t * 2) * 0.02;
    girl.rotation.y = Math.sin(t * 0.7) * 0.12;

    // Wave strength: eases in, holds, eases out
    let amt = 0;
    if (waveT >= 0) {
      waveT += dt;
      if (waveT > DUR) waveT = -1;
      else amt = Math.min(1, waveT / 0.35) * Math.min(1, (DUR - waveT) / 0.35);
    }
    head.rotation.z = Math.sin(t * 1.3) * 0.04 + amt * 0.1;

    armR.shoulder.rotation.z = 0.12 + (2.65 - 0.12) * amt;
    armR.elbow.rotation.z = 0.1 + Math.sin(Math.max(waveT, 0) * 14) * 0.6 * amt;
    armL.shoulder.rotation.z = -0.12 + Math.sin(t * 2) * 0.02;

    tails.forEach(g => {
      g.rotation.z = g.userData.s * (0.05 + Math.sin(t * 2.2 + g.userData.s) * 0.08 + amt * 0.05);
    });

    // Blink
    const p = (t - nextBlink) / 0.18;
    let sy = 1.25;
    if (p >= 0 && p < 1) sy = 1.25 * (1 - 0.9 * Math.sin(p * Math.PI));
    else if (p >= 1) nextBlink = t + 2 + Math.random() * 3;
    eyes.forEach(e => { e.scale.y = sy; });

    renderer.render(scene, camera);
  }
  frame();
})();
</script>
<!-- GIRL WIDGET END -->

</body>
</html>
