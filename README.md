<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Votación de Cuadros</title>

  <!-- Fuente Inter -->
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet" />
  <!-- Material Symbols -->
  <link href="https://fonts.googleapis.com/css2?family=Material+Symbols+Rounded" rel="stylesheet" />

  <style>
    :root {
      --bg-light: #f5f5f7;
      --text-light: #1c1c1e;
      --accent: #007aff;
      --accent-hover: #0059c9;
      --shadow: 0 10px 30px rgba(0, 0, 0, 0.12);
      --radius: 22px;
    }

    * { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      font-family: "Inter", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
      background: var(--bg-light);
      color: var(--text-light);
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      overflow: hidden;
    }

    .frame {
      position: relative;
      width: 90%;
      max-width: 900px;
      aspect-ratio: 7 / 10;
      background: white;
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      overflow: hidden;
      display: flex;
      align-items: center;
      justify-content: center;
      margin: auto;
    }

    .slider {
      display: flex;
      transition: transform 0.4s ease-in-out;
      height: 100%;
      width: 100%;
      pointer-events: none;
      opacity: 0.6;
      filter: blur(2px);
      transition: all 0.3s ease;
    }

    .slider.active {
      pointer-events: auto;
      opacity: 1;
      filter: none;
    }

    .slide {
      min-width: 100%;
      position: relative;
      display: flex;
      align-items: center;
      justify-content: center;
      overflow: hidden;
      background: #f8f8fa;
    }

    .slide img {
      width: 100%;
      height: 100%;
      object-fit: contain;
      transition: transform 0.3s ease;
      cursor: pointer;
    }

    .slide img:hover { transform: scale(1.01); }

    .counter {
      position: absolute;
      top: 24px;
      left: 50%;
      transform: translateX(-50%);
      background: rgba(0, 0, 0, 0.75);
      color: white;
      padding: 14px 26px;
      border-radius: 999px;
      font-weight: 700;
      font-size: 1.3rem;
      letter-spacing: 0.3px;
      backdrop-filter: blur(6px);
      box-shadow: 0 6px 20px rgba(0, 0, 0, 0.3);
    }

    .vote-btn {
      position: absolute;
      bottom: 38px;
      left: 50%;
      transform: translateX(-50%);
      background: var(--accent);
      color: white;
      border: none;
      padding: 26px 80px;
      border-radius: 999px;
      font-size: 2rem;
      font-weight: 700;
      cursor: pointer;
      box-shadow: 0 15px 35px rgba(0, 122, 255, 0.4);
      transition: all 0.25s ease;
      letter-spacing: 0.5px;
    }

    .vote-btn:hover {
      background: var(--accent-hover);
      transform: translateX(-50%) scale(1.05);
      box-shadow: 0 20px 40px rgba(0, 122, 255, 0.55);
    }

    .vote-btn:disabled {
      background: #c9c9c9;
      cursor: not-allowed;
      box-shadow: none;
      transform: translateX(-50%) scale(1);
    }

    .nav-btn {
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
      background: rgba(255, 255, 255, 0.9);
      border: none;
      color: var(--text-light);
      width: 70px;
      height: 70px;
      border-radius: 50%;
      cursor: pointer;
      box-shadow: 0 6px 20px rgba(0, 0, 0, 0.15);
      transition: all 0.2s ease;
      backdrop-filter: blur(10px);
      display: flex;
      align-items: center;
      justify-content: center;
    }

    .nav-btn:hover {
      background: var(--accent);
      color: white;
      transform: translateY(-50%) scale(1.08);
      box-shadow: 0 10px 30px rgba(0, 122, 255, 0.4);
    }

    .material-symbols-rounded {
      font-variation-settings: 'FILL' 1,'wght' 600,'GRAD' 0,'opsz' 48;
      font-size: 36px;
      line-height: 1;
      pointer-events: none;
    }

    #prevBtn { left: 18px; }
    #nextBtn { right: 18px; }

    .modal {
      position: fixed;
      inset: 0;
      background: rgba(0, 0, 0, 0.85);
      display: none;
      align-items: center;
      justify-content: center;
      z-index: 10;
      animation: fadeIn 0.3s ease;
    }

    .modal.open { display: flex; }

    .modal img {
      max-width: 95%;
      max-height: 90%;
      border-radius: 16px;
      box-shadow: 0 15px 40px rgba(0, 0, 0, 0.3);
      animation: zoomIn 0.3s ease;
    }

    @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
    @keyframes zoomIn { from { transform: scale(0.96); opacity: 0.8; } to { transform: scale(1); opacity: 1; } }

    .intro-modal {
      position: fixed;
      inset: 0;
      display: flex;
      align-items: center;
      justify-content: center;
      background: rgba(0, 0, 0, 0.45);
      backdrop-filter: blur(6px);
      z-index: 50;
      animation: fadeIn 0.4s ease;
    }

    .intro-box {
      background: white;
      color: var(--text-light);
      padding: 60px 70px;
      border-radius: 24px;
      box-shadow: var(--shadow);
      text-align: center;
      max-width: 520px;
      width: 90%;
      animation: zoomIn 0.3s ease;
    }

    .intro-box h2 {
      font-size: 1.8rem;
      margin-bottom: 28px;
      font-weight: 700;
      color: #111;
    }

    .intro-box p {
      font-size: 1.1rem;
      margin-bottom: 40px;
      color: #333;
      line-height: 1.6;
    }

    .intro-box button {
      background: var(--accent);
      color: white;
      border: none;
      font-size: 1.1rem;
      font-weight: 600;
      padding: 14px 36px;
      border-radius: 999px;
      cursor: pointer;
      transition: all 0.2s ease;
      box-shadow: 0 6px 20px rgba(0, 122, 255, 0.3);
    }

    .intro-box button:hover {
      background: var(--accent-hover);
      transform: scale(1.05);
    }

    @media (max-width: 600px) {
      .intro-box {
        padding: 40px 30px;
      }
      .intro-box h2 {
        font-size: 1.4rem;
        margin-bottom: 20px;
      }
      .intro-box p {
        font-size: 1rem;
        margin-bottom: 25px;
      }
      .intro-box button {
        padding: 12px 28px;
        font-size: 1rem;
      }
    }
  </style>
</head>

<body>
  <div class="intro-modal" id="introModal">
    <div class="intro-box">
      <h2>Bienvenido a la votación</h2>
      <p>Selecciona tu cuadro favorito y emite tu voto.<br>Solo puedes votar una vez.</p>
      <button id="acceptBtn">Aceptar</button>
    </div>
  </div>

  <div class="frame">
    <div class="slider" id="slider"></div>

    <button id="prevBtn" class="nav-btn" aria-label="Anterior">
      <span class="material-symbols-rounded">arrow_back_ios_new</span>
    </button>

    <button id="nextBtn" class="nav-btn" aria-label="Siguiente">
      <span class="material-symbols-rounded">arrow_forward_ios</span>
    </button>
  </div>

  <div class="modal" id="modal">
    <img id="modalImg" alt="Vista previa" />
  </div>

  <script type="module">
    import { initializeApp } from 'https://www.gstatic.com/firebasejs/10.13.0/firebase-app.js';
    import { getDatabase, ref, onValue, runTransaction } from 'https://www.gstatic.com/firebasejs/10.13.0/firebase-database.js';

    const firebaseConfig = {
      apiKey: "AIzaSyCAgMXWmtyyF4OK15pUoN9cHgfF2xlUPv0",
      authDomain: "votaciones-3a70a.firebaseapp.com",
      databaseURL: "https://votaciones-3a70a-default-rtdb.firebaseio.com",
      projectId: "votaciones-3a70a",
      storageBucket: "votaciones-3a70a.firebasestorage.app",
      messagingSenderId: "498536197257",
      appId: "1:498536197257:web:5158a87b4e53fa7f64ea58"
    };

    const app = initializeApp(firebaseConfig);
    const db = getDatabase(app);

    const cuadros = Array.from({ length: 7 }, (_, i) => ({
      id: `${i + 1}`,
      src: `imagenes/${i + 1}.jpg`
    }));

    const slider = document.getElementById("slider");
    cuadros.forEach(c => {
      const slide = document.createElement("div");
      slide.className = "slide";
      slide.innerHTML = `
        <img src="${c.src}" alt="Cuadro ${c.id}" data-id="${c.id}">
        <div class="counter" id="count-${c.id}">0 votos</div>
        <button class="vote-btn" data-id="${c.id}">VOTAR</button>
      `;
      slider.appendChild(slide);
    });

    let index = 0;
    const nextBtn = document.getElementById("nextBtn");
    const prevBtn = document.getElementById("prevBtn");
    const modal = document.getElementById("modal");
    const modalImg = document.getElementById("modalImg");

    function showSlide(i) {
      index = (i + cuadros.length) % cuadros.length;
      slider.style.transform = `translateX(-${index * 100}%)`;
    }

    nextBtn.addEventListener("click", () => showSlide(index + 1));
    prevBtn.addEventListener("click", () => showSlide(index - 1));

    slider.addEventListener("click", e => {
      if (e.target.tagName === "IMG") {
        modalImg.src = e.target.src;
        modal.classList.add("open");
      }
    });
    modal.addEventListener("click", () => modal.classList.remove("open"));

    // 🔹 Control de votación única
    const userVote = localStorage.getItem("userVote");

    function disableAllButtons() {
      document.querySelectorAll(".vote-btn").forEach(btn => {
        btn.disabled = true;
      });
    }

    if (userVote) disableAllButtons();

    slider.addEventListener("click", async e => {
      if (e.target.classList.contains("vote-btn")) {
        if (localStorage.getItem("userVote")) {
          alert("Ya has votado por el cuadro #" + localStorage.getItem("userVote") + ". Solo puedes votar una vez.");
          return;
        }
        const id = e.target.dataset.id;
        const voteRef = ref(db, `cuadroVotes/${id}`);
        await runTransaction(voteRef, n => (n || 0) + 1);
        localStorage.setItem("userVote", id);
        disableAllButtons();
        alert("Tu voto por el cuadro #" + id + " ha sido registrado, gracias por participar.");
      }
    });

    // Contadores
    cuadros.forEach(c => {
      const r = ref(db, `cuadroVotes/${c.id}`);
      onValue(r, snap => {
        const val = snap.val() || 0;
        const counter = document.getElementById(`count-${c.id}`);
        if (counter) counter.textContent = `${val} voto${val !== 1 ? 's' : ''}`;
      });
    });

    // Modal inicial
    const introModal = document.getElementById("introModal");
    const acceptBtn = document.getElementById("acceptBtn");

    acceptBtn.addEventListener("click", () => {
      introModal.style.display = "none";
      slider.classList.add("active");
    });
  </script>
</body>
</html>
