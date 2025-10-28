<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Votación de Cuadros</title>
  <link rel="shortcut icon" type="image/x-icon" href="favicon.ico">
  <style>
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
      padding: 20px;
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
    .info-message:hover { background: #e0e7ff; }

    .highlight {
      font-weight: bold;
      color: #dc2626;
      margin-top: 8px;
      display: block;
    }

    /* ======= CUADROS ======= */
    .cuadros-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
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
      position: relative;
    }

    .cuadro-item:hover {
      transform: translateY(-6px);
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

    .cuadro-details {
      padding: 15px 20px 20px;
    }

    .cuadro-item h3 {
      font-size: 1.15em;
      color: var(--color-primary);
      margin-bottom: 5px;
      text-align: center;
    }

    .curso {
      color: #4b5563;
      font-size: 0.9em;
      font-style: italic;
      margin-bottom: 8px;
      text-align: center;
    }

    .cuadro-item p {
      color: #6b7280;
      font-size: 0.9em;
      margin-bottom: 15px;
      text-align: center;
    }

    .vote-info {
      text-align: center;
    }

    .vote-button {
      background: var(--color-success);
      border: none;
      color: #fff;
      padding: 12px 25px;
      border-radius: var(--radius);
      font-weight: 600;
      cursor: pointer;
      transition: var(--transition);
      width: 100%;
      max-width: 200px;
      margin: auto;
      display: block;
      font-size: 1em;
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
      padding: 20px;
    }

    .modal-content {
      background: var(--color-card);
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      max-width: 100%;
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

    /* ======= MEDIA QUERIES ======= */
    @media (max-width: 768px) {
      .container {
        padding: 25px 20px;
      }
      h1 {
        font-size: 1.7em;
      }
      .info-message {
        font-size: 0.9em;
        padding: 12px;
      }
      .cuadro-item img {
        height: 200px;
      }
    }

    @media (max-width: 480px) {
      body {
        padding: 10px;
      }
      .container {
        padding: 20px 15px;
        border-radius: 10px;
      }
      h1 {
        font-size: 1.5em;
      }
      .info-message {
        font-size: 0.85em;
      }
      .cuadro-item img {
        height: 180px;
      }
      .vote-button {
        padding: 10px;
        font-size: 0.95em;
      }
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Votación</h1>
    <p class="info-message">
      Selecciona tu cuadro favorito y emite tu voto.
      <b>Solo puedes votar una vez.</b>
    </p>

    <div class="cuadros-grid">
      <div class="cuadro-item" data-fullsrc="imagenes/1.jpg" data-caption="{{titulo1}} - Por {{autor1}}">
        <img src="imagenes/1.jpg" alt="Nombre">
        <div class="cuadro-details">
          <h3>Nombre</h3>
          <div class="curso">Curso: 1101</div>
          <p>Descripción</p>
          <div class="vote-info">
            <button class="vote-button" data-cuadro-id="1">Votar</button>
            <span class="vote-count" id="votes-1">0 votos</span>
          </div>
        </div>
      </div>

      <div class="cuadro-item" data-fullsrc="imagenes/2.jpg" data-caption="{{titulo2}} - Por {{autor2}}">
        <img src="imagenes/2.jpg" alt="Nombre">
        <div class="cuadro-details">
          <h3>Nombre</h3>
          <div class="curso">Curso: </div>
          <p>Descripción</p>
          <div class="vote-info">
            <button class="vote-button" data-cuadro-id="2">Votar</button>
            <span class="vote-count" id="votes-2">0 votos</span>
          </div>
        </div>
      </div>

      <div class="cuadro-item" data-fullsrc="imagenes/3.jpg" data-caption="{{titulo3}} - Por {{autor3}}">
        <img src="imagenes/3.jpg" alt="Nombre">
        <div class="cuadro-details">
          <h3>Nombre</h3>
          <div class="curso">Curso: </div>
          <p>Descripción</p>
          <div class="vote-info">
            <button class="vote-button" data-cuadro-id="3">Votar</button>
            <span class="vote-count" id="votes-3">0 votos</span>
          </div>
        </div>
      </div>

      <div class="cuadro-item" data-fullsrc="imagenes/4.jpg" data-caption="{{titulo4}} - Por {{autor4}}">
        <img src="imagenes/4.jpg" alt="Nombre">
        <div class="cuadro-details">
          <h3>Nombre</h3>
          <div class="curso">Curso: </div>
          <p>Descripción</p>
          <div class="vote-info">
            <button class="vote-button" data-cuadro-id="4">Votar</button>
            <span class="vote-count" id="votes-4">0 votos</span>
          </div>
        </div>
      </div>

      <div class="cuadro-item" data-fullsrc="imagenes/5.jpg" data-caption="{{titulo5}} - Por {{autor5}}">
        <img src="imagenes/5.jpg" alt="Nombre">
        <div class="cuadro-details">
          <h3>Nombre</h3>
          <div class="curso">Curso: </div>
          <p>Descripción</p>
          <div class="vote-info">
            <button class="vote-button" data-cuadro-id="5">Votar</button>
            <span class="vote-count" id="votes-5">0 votos</span>
          </div>
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
