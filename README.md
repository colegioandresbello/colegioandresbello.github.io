<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Votación de Cuadros - Colegio Andrés Bello</title>
  <style>
    /* ======= ESTILOS ======= */
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 20px;
      background-color: #f4f4f4;
      color: #333;
      line-height: 1.6;
    }
    .container {
      max-width: 900px;
      margin: auto;
      background: #fff;
      padding: 30px;
      border-radius: 8px;
      box-shadow: 0 2px 10px rgba(0,0,0,0.1);
      box-sizing: border-box;
    }
    h1, h2 {
      text-align: center;
      color: #0056b3;
      margin-bottom: 20px;
    }
    h1 { font-size: 2.2em; }
    h2 { font-size: 1.6em; }
    .info-message {
      text-align: center;
      margin: 15px 0 25px 0;
    }
    .highlight {
      font-weight: bold;
      color: #dc3545;
      display: block;
      margin-top: 5px;
    }

    /* Cuadros */
    .cuadros-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
      gap: 20px;
      margin-top: 30px;
      justify-content: center;
    }
    .cuadro-item {
      border: 1px solid #ddd;
      border-radius: 8px;
      overflow: hidden;
      text-align: center;
      background: #fff;
      box-shadow: 0 1px 5px rgba(0,0,0,0.05);
      cursor: pointer;
      transition: transform 0.2s;
      position: relative;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
    }
    .cuadro-item:hover { transform: translateY(-5px); }
    .cuadro-item img {
      width: 100%;
      height: 200px;
      object-fit: cover;
      display: block;
    }
    .cuadro-item h3 {
      margin: 15px 0 10px;
      color: #333;
      font-size: 1.2em;
      padding: 0 15px;
    }
    .cuadro-item p {
      font-size: 0.95em;
      color: #666;
      padding: 0 15px;
      flex-grow: 1;
    }
    .vote-info { padding-bottom: 15px; }
    .vote-button {
      background-color: #28a745;
      color: white;
      padding: 10px 15px;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      font-size: 1em;
      margin: 10px 0 0;
      transition: background-color 0.2s;
      width: calc(100% - 30px);
      max-width: 200px;
    }
    .vote-button:hover:not(.disabled) { background-color: #218838; }
    .vote-button.disabled {
      background-color: #cccccc;
      cursor: not-allowed;
    }
    .vote-count {
      position: absolute;
      top: 10px;
      right: 10px;
      background-color: rgba(0,0,0,0.7);
      color: white;
      padding: 5px 10px;
      border-radius: 5px;
      font-size: 0.85em;
      font-weight: bold;
    }

    /* Modal */
    .modal {
      display: none;
      position: fixed;
      z-index: 1000;
      left: 0; top: 0;
      width: 100%; height: 100%;
      background-color: rgba(0,0,0,0.7);
      justify-content: center;
      align-items: center;
      overflow-y: auto;
    }
    .modal-content {
      margin: 20px;
      width: 90%;
      max-width: 700px;
      background: #fff;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 4px 20px rgba(0,0,0,0.3);
      position: relative;
    }
    .modal-content img {
      width: 100%;
      height: auto;
      border-radius: 5px;
    }
    .modal-caption {
      margin-top: 15px;
      text-align: center;
      color: #555;
    }
    .close {
      position: absolute;
      top: 10px; right: 15px;
      color: #aaa;
      font-size: 3em;
      font-weight: bold;
      transition: 0.3s;
    }
    .close:hover { color: #333; cursor: pointer; }
  </style>
</head>
<body>
  <div class="container">
    <h1>Votación de Cuadros</h1>
    <h2>Colegio Andrés Bello</h2>
    <p class="info-message">
      ¡Bienvenido/a! Selecciona tu cuadro favorito haciendo clic en la imagen para previsualizarlo y luego haz clic en "Votar" para emitir tu voto.
      <span class="highlight">Solo puedes votar una única vez.</span>
    </p>

    <div class="cuadros-grid">
      <div class="cuadro-item" data-fullsrc="imagenes/1.jpg" data-caption="El Amanecer del Cóndor - Por [Artista 1]">
        <img src="imagenes/1.jpg" alt="El Amanecer del Cóndor">
        <h3>El Amanecer del Cóndor</h3>
        <p>Una obra maestra que captura la majestuosidad de los Andes al amanecer.</p>
        <div class="vote-info">
          <button class="vote-button" data-cuadro-id="1">Votar</button>
          <span class="vote-count" id="votes-1">0 votos</span>
        </div>
      </div>

      <div class="cuadro-item" data-fullsrc="imagenes/2.jpg" data-caption="Naturaleza Viva - Por [Artista 2]">
        <img src="imagenes/2.jpg" alt="Naturaleza Viva">
        <h3>Naturaleza Viva</h3>
        <p>Una explosión de colores que celebra la biodiversidad de nuestra región.</p>
        <div class="vote-info">
          <button class="vote-button" data-cuadro-id="2">Votar</button>
          <span class="vote-count" id="votes-2">0 votos</span>
        </div>
      </div>

      <div class="cuadro-item" data-fullsrc="imagenes/3.jpg" data-caption="Rostros de Nuestra Gente - Por [Artista 3]">
        <img src="imagenes/3.jpg" alt="Rostros de Nuestra Gente">
        <h3>Rostros de Nuestra Gente</h3>
        <p>Retratos que reflejan la riqueza cultural y las historias de nuestra comunidad.</p>
        <div class="vote-info">
          <button class="vote-button" data-cuadro-id="3">Votar</button>
          <span class="vote-count" id="votes-3">0 votos</span>
        </div>
      </div>

      <div class="cuadro-item" data-fullsrc="imagenes/4.jpg" data-caption="Abstracto Urbano - Por [Artista 4]">
        <img src="imagenes/4.jpg" alt="Abstracto Urbano">
        <h3>Abstracto Urbano</h3>
        <p>Una interpretación moderna de la energía y el dinamismo de la ciudad.</p>
        <div class="vote-info">
          <button class="vote-button" data-cuadro-id="4">Votar</button>
          <span class="vote-count" id="votes-4">0 votos</span>
        </div>
      </div>

      <div class="cuadro-item" data-fullsrc="imagenes/5.jpg" data-caption="Serenidad Marina - Por [Artista 5]">
        <img src="imagenes/5.jpg" alt="Serenidad Marina">
        <h3>Serenidad Marina</h3>
        <p>Una vista relajante del océano al atardecer, capturando su calma infinita.</p>
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

  <!-- 🔥 Firebase SDKs -->
  <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-database-compat.js"></script>

  <script>
    // ============================================================
    // 🚀 CONFIGURA AQUÍ TU FIREBASE
    // (copia tu firebaseConfig desde la consola de Firebase)
    // ============================================================
    const firebaseConfig = {
  apiKey: "AIzaSyCAgMXWmtyyF4OK15pUoN9cHgfF2xlUPv0",
  authDomain: "votaciones-3a70a.firebaseapp.com",
  projectId: "votaciones-3a70a",
  storageBucket: "votaciones-3a70a.firebasestorage.app",
  messagingSenderId: "498536197257",
  appId: "1:498536197257:web:5158a87b4e53fa7f64ea58"
};

    // Inicializar Firebase
    firebase.initializeApp(firebaseConfig);
    const database = firebase.database();
    const votesRef = database.ref('cuadroVotes');

    // Variable local para saber si el usuario ya votó
    let userHasVoted = localStorage.getItem('userHasVoted') === 'true';

    // Actualiza el conteo en la pantalla
    function updateVoteCounts(votes) {
      for (const id in votes) {
        const span = document.getElementById(`votes-${id}`);
        if (span) span.textContent = `${votes[id]} votos`;
      }
    }

    // Desactiva botones
    function disableAllVoteButtons() {
      const buttons = document.getElementsByClassName("vote-button");
      for (let btn of buttons) {
        btn.classList.add("disabled");
        btn.disabled = true;
      }
    }

    // Escuchar cambios en tiempo real
    votesRef.on('value', (snapshot) => {
      const data = snapshot.val() || {1:0,2:0,3:0,4:0,5:0};
      updateVoteCounts(data);
    });

    // Si ya votó, desactivar al cargar
    document.addEventListener('DOMContentLoaded', () => {
      if (userHasVoted) disableAllVoteButtons();
    });

    // Modal de previsualización
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

    // Votación
    const voteButtons = document.getElementsByClassName("vote-button");
    for (let btn of voteButtons) {
      btn.onclick = function() {
        if (userHasVoted) {
          alert("Ya has emitido tu voto. No puedes votar de nuevo.");
          return;
        }
        const id = this.getAttribute("data-cuadro-id");
        votesRef.child(id).transaction(current => (current || 0) + 1);
        userHasVoted = true;
        localStorage.setItem('userHasVoted', 'true');
        disableAllVoteButtons();
        alert("¡Voto registrado por el Cuadro " + id + "! ¡Gracias por participar!");
      };
    }
  </script>
</body>
</html>
