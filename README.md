<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Votación de Cuadros</title>
  <style>
    /* ======= BASE ======= */
    :root {
      --color-primary: #2563eb;
      --color-success: #10b981;
      --color-bg: #f9fafb;
      --color-card: #ffffff;
      --color-text: #111827;
      --radius: 14px;
      --shadow: 0 8px 24px rgba(0, 0, 0, 0.08);
      --transition: all 0.3s ease;
    }
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }
    body {
      font-family: "Inter", Arial, sans-serif;
      background-color: var(--color-bg);
      color: var(--color-text);
      padding: 30px 15px;
      line-height: 1.6;
    }

    .container {
      max-width: 960px;
      margin: auto;
      background: var(--color-card);
      padding: 40px;
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      animation: fadeIn 0.7s ease-in-out;
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(10px); }
      to { opacity: 1; transform: translateY(0); }
    }

    h1 {
      text-align: center;
      color: var(--color-primary);
      font-weight: 700;
      font-size: 2em;
      margin-bottom: 5px;
    }
    h2 {
      text-align: center;
      color: #6b7280;
      font-weight: 400;
      font-size: 1.1em;
      margin-bottom: 25px;
    }

    .info-message {
      text-align: center;
      background: #eef2ff;
      color: #4338ca;
      padding: 15px;
      border-radius: var(--radius);
      font-size: 0.95em;
      box-shadow: 0 2px 6px rgba(37, 99, 235, 0.1);
      margin-bottom: 30px;
      transition: var(--transition);
    }
    .info-message:hover {
      background: #e0e7ff;
    }

    .highlight {
      font-weight: bold;
      color: #dc2626;
      margin-top: 8px;
      display: block;
    }

    /* ======= CUADROS ======= */
    .cuadros-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
      gap: 25px;
      justify-content: center;
    }

    .cuadro-item {
      background: var(--color-card);
      border-radius: var(--radius);
      overflow: hidden;
      box-shadow: var(--shadow);
      transition: var(--transition);
      cursor: pointer;
      transform: translateY(0);
    }

    .cuadro-item:hover {
      transform: translateY(-5px);
      box-shadow: 0 12px 32px rgba(0, 0, 0, 0.1);
    }

    .cuadro-item img {
      width: 100%;
      height: 220px;
      object-fit: cover;
      transition: transform 0.4s ease;
    }

    .cuadro-item:hover img {
      transform: scale(1.05);
    }

    .cuadro-item h3 {
      font-size: 1.1em;
      color: var(--color-primary);
      margin: 15px 0 5px;
    }

    .cuadro-item p {
      color: #6b7280;
      font-size: 0.9em;
      padding: 0 15px 10px;
    }

    .vote-info {
      text-align: center;
      padding: 10px 0 20px;
    }

    .vote-button {
      background: var(--color-success);
      border: none;
      color: #fff;
      padding: 10px 20px;
      border-radius: var(--radius);
      font-weight: 600;
      cursor: pointer;
      transition: var(--transition);
    }

    .vote-button:hover:not(.disabled) {
      background: #059669;
      transform: scale(1.05);
    }

    .vote-button.disabled {
      background: #d1d5db;
      cursor: not-allowed;
    }

    .vote-count {
      position: absolute;
      top: 10px;
      right: 10px;
      background: rgba(17, 24, 39, 0.8);
      color: white;
      padding: 5px 10px;
      border-radius: 20px;
      font-size: 0.8em;
      font-weight: bold;
      opacity: 0.9;
      transition: var(--transition);
    }

    /* ======= MODAL ======= */
    .modal {
      display: none;
      position: fixed;
      z-index: 999;
      left: 0;
      top: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.7);
      justify-content: center;
      align-items: center;
      animation: fadeIn 0.4s ease;
    }

    .modal-content {
      background: var(--color-card);
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      max-width: 90%;
      width: 600px;
      padding: 20px;
      animation: zoomIn 0.4s ease;
    }

    @keyframes zoomIn {
      from { transform: scale(0.8); opacity: 0; }
      to { transform: scale(1); opacity: 1; }
    }

    .modal-content img {
      width: 100%;
      border-radius: var(--radius);
    }

    .modal-caption {
      text-align: center;
      margin-top: 15px;
      color: #374151;
      font-size: 1em;
    }

    .close {
      position: absolute;
      top: 25px;
      right: 35px;
      color: white;
      font-size: 2em;
      cursor: pointer;
      transition: var(--transition);
    }

    .close:hover { color: #f87171; }
  </style>
</head>
<body>
  <div class="container">
    <h1>Votación de Cuadros</h1>
    <h2>Colegio Andrés Bello</h2>
    <p class="info-message">
      Selecciona tu cuadro favorito y emite tu voto.
      <span class="highlight">Solo puedes votar una vez.</span>
    </p>

    <div class="cuadros-grid">
      <div class="cuadro-item" data-fullsrc="imagenes/1.jpg" data-caption="El Amanecer del Cóndor - Por [Artista 1]">
        <img src="imagenes/1.jpg" alt="El Amanecer del Cóndor">
        <h3>El Amanecer del Cóndor</h3>
        <p>Captura la majestuosidad de los Andes al amanecer.</p>
        <div class="vote-info">
          <button class="vote-button" data-cuadro-id="1">Votar</button>
          <span class="vote-count" id="votes-1">0 votos</span>
        </div>
      </div>

      <div class="cuadro-item" data-fullsrc="imagenes/2.jpg" data-caption="Naturaleza Viva - Por [Artista 2]">
        <img src="imagenes/2.jpg" alt="Naturaleza Viva">
        <h3>Naturaleza Viva</h3>
        <p>Una explosión de color y biodiversidad.</p>
        <div class="vote-info">
          <button class="vote-button" data-cuadro-id="2">Votar</button>
          <span class="vote-count" id="votes-2">0 votos</span>
        </div>
      </div>

      <div class="cuadro-item" data-fullsrc="imagenes/3.jpg" data-caption="Rostros de Nuestra Gente - Por [Artista 3]">
        <img src="imagenes/3.jpg" alt="Rostros de Nuestra Gente">
        <h3>Rostros de Nuestra Gente</h3>
        <p>Retratos que celebran nuestra cultura.</p>
        <div class="vote-info">
          <button class="vote-button" data-cuadro-id="3">Votar</button>
          <span class="vote-count" id="votes-3">0 votos</span>
        </div>
      </div>

      <div class="cuadro-item" data-fullsrc="imagenes/4.jpg" data-caption="Abstracto Urbano - Por [Artista 4]">
        <img src="imagenes/4.jpg" alt="Abstracto Urbano">
        <h3>Abstracto Urbano</h3>
        <p>Una mirada moderna a la energía de la ciudad.</p>
        <div class="vote-info">
          <button class="vote-button" data-cuadro-id="4">Votar</button>
          <span class="vote-count" id="votes-4">0 votos</span>
        </div>
      </div>

      <div class="cuadro-item" data-fullsrc="imagenes/5.jpg" data-caption="Serenidad Marina - Por [Artista 5]">
        <img src="imagenes/5.jpg" alt="Serenidad Marina">
        <h3>Serenidad Marina</h3>
        <p>El océano al atardecer en calma infinita.</p>
        <div class="vote-info">
          <button class="vote-button" data-cuadro-id="5">Votar</button>
          <span class="vote-count" id="votes-5">0 votos</span>
        </div>
      </div>
    </div>
  </div>

  <!-- Modal -->
  <div id="myModal" class="modal">
    <span class="close">&times;</span>
    <div class="modal-content">
      <img class="modal-img" id="img01" src="">
      <div id="caption" class="modal-caption"></div>
    </div>
  </div>

  <!-- Firebase SDK -->
  <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-database-compat.js"></script>
  <script>
    const firebaseConfig = {
      apiKey: "AIzaSyCAgMXWmtyyF4OK15pUoN9cHgfF2xlUPv0",
      authDomain: "votaciones-3a70a.firebaseapp.com",
      databaseURL: "https://votaciones-3a70a-default-rtdb.firebaseio.com",
      projectId: "votaciones-3a70a",
      storageBucket: "votaciones-3a70a.appspot.com",
      messagingSenderId: "498536197257",
      appId: "1:498536197257:web:5158a87b4e53fa7f64ea58"
    };

    firebase.initializeApp(firebaseConfig);
    const database = firebase.database();
    const votesRef = database.ref('cuadroVotes');
    let userHasVoted = localStorage.getItem('userHasVoted') === 'true';

    function updateVoteCounts(votes) {
      for (const id in votes) {
        const span = document.getElementById(`votes-${id}`);
        if (span) span.textContent = `${votes[id]} votos`;
      }
    }

    function disableAllVoteButtons() {
      const buttons = document.getElementsByClassName("vote-button");
      for (let btn of buttons) {
        btn.classList.add("disabled");
        btn.disabled = true;
      }
    }

    votesRef.on('value', (snapshot) => {
      const data = snapshot.val() || {1:0,2:0,3:0,4:0,5:0};
      updateVoteCounts(data);
    });

    document.addEventListener('DOMContentLoaded', () => {
      if (userHasVoted) disableAllVoteButtons();
    });

    const modal = document.getElementById("myModal");
    const modalImg = document.getElementById("img01");
    const captionText = document.getElementById("caption");
    const cuadros = document.getElementsByClassName("cuadro-item");

    for (let cuadro of cuadros) {
      cuadro.querySelector("img").onclick = function() {
        modal.style.display = "flex";
        modalImg.src = cuadro.getAttribute("data-fullsrc");
        captionText.innerHTML = cuadro.getAttribute("data-caption");
      };
    }

    document.getElementsByClassName("close")[0].onclick = () => modal.style.display = "none";
    modal.onclick = (e) => { if (e.target == modal) modal.style.display = "none"; };

    const voteButtons = document.getElementsByClassName("vote-button");
    for (let btn of voteButtons) {
      btn.onclick = function() {
        if (userHasVoted) {
          alert("Ya has emitido tu voto.");
          return;
        }
        const id = this.getAttribute("data-cuadro-id");
        votesRef.child(id).transaction(current => (current || 0) + 1);
        userHasVoted = true;
        localStorage.setItem('userHasVoted', 'true');
        disableAllVoteButtons();
        alert("¡Voto registrado! Gracias por participar.");
      };
    }
  </script>
</body>
</html>
