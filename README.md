<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Votación de Cuadros - Colegio Andrés Bello</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
            background-color: #f4f4f4;
            color: #333;
        }
        .container {
            max-width: 900px;
            margin: auto;
            background: #fff;
            padding: 30px;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        h1, h2 {
            text-align: center;
            color: #0056b3;
        }
        .cuadros-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-top: 30px;
        }
        .cuadro-item {
            border: 1px solid #ddd;
            border-radius: 8px;
            overflow: hidden;
            text-align: center;
            background: #fff;
            box-shadow: 0 1px 5px rgba(0,0,0,0.05);
            cursor: pointer; /* Indica que es clicable */
            transition: transform 0.2s;
            position: relative; /* Para posicionar el contador de votos */
        }
        .cuadro-item:hover {
            transform: translateY(-5px);
        }
        .cuadro-item img {
            width: 100%;
            height: 200px; /* Altura fija para las miniaturas */
            object-fit: cover; /* Asegura que la imagen cubra el área sin distorsionarse */
            display: block;
        }
        .cuadro-item h3 {
            margin: 15px 0 10px;
            color: #333;
        }
        .cuadro-item p {
            font-size: 0.9em;
            color: #666;
            padding: 0 15px;
        }
        .vote-button {
            background-color: #28a745;
            color: white;
            padding: 10px 15px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            margin: 10px 0 15px; /* Ajuste de margen */
            transition: background-color 0.2s;
        }
        .vote-button:hover:not(.disabled) { /* Solo hover si no está deshabilitado */
            background-color: #218838;
        }
        .vote-button.disabled { /* Estilo para botón deshabilitado */
            background-color: #cccccc;
            cursor: not-allowed;
        }
        .vote-count {
            position: absolute;
            top: 10px;
            right: 10px;
            background-color: rgba(0, 0, 0, 0.7);
            color: white;
            padding: 5px 10px;
            border-radius: 5px;
            font-size: 0.9em;
            font-weight: bold;
        }

        /* Estilos para el Modal de Previsualización */
        .modal {
            display: none; /* Oculto por defecto */
            position: fixed; /* Posición fija para cubrir toda la pantalla */
            z-index: 1; /* Puesto encima de todo */
            left: 0;
            top: 0;
            width: 100%; /* Ancho completo */
            height: 100%; /* Alto completo */
            overflow: auto; /* Habilitar scroll si es necesario */
            background-color: rgba(0,0,0,0.7); /* Fondo oscuro semitransparente */
            justify-content: center;
            align-items: center;
        }
        .modal-content {
            margin: auto;
            display: block;
            width: 80%;
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
            display: block;
            border-radius: 5px;
        }
        .modal-caption {
            margin-top: 10px;
            text-align: center;
            color: #555;
            font-size: 1.1em;
        }
        .close {
            position: absolute;
            top: 15px;
            right: 25px;
            color: #aaa;
            font-size: 40px;
            font-weight: bold;
            transition: 0.3s;
        }
        .close:hover,
        .close:focus {
            color: #333;
            text-decoration: none;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Votación de Cuadros</h1>
        <h2>Colegio Andrés Bello</h2>
        <p style="text-align: center;">¡Bienvenido/a! Selecciona tu cuadro favorito haciendo clic en la imagen para previsualizarlo y luego haz clic en "Votar" para emitir tu voto. <br> <span style="font-weight: bold; color: #dc3545;">Solo puedes votar una única vez.</span></p>

        <div class="cuadros-grid">
            <!-- Cuadro 1 -->
            <div class="cuadro-item" data-fullsrc="imagenes/1.jpg" data-caption="El Amanecer del Cóndor - Por [Artista 1]">
                <img src="imagenes/1.jpg" alt="El Amanecer del Cóndor">
                <h3>El Amanecer del Cóndor</h3>
                <p>Una obra maestra que captura la majestuosidad de los Andes al amanecer.</p>
                <div class="vote-info">
                    <button class="vote-button" data-cuadro-id="1">Votar</button>
                    <span class="vote-count" id="votes-1">0 votos</span>
                </div>
            </div>

            <!-- Cuadro 2 -->
            <div class="cuadro-item" data-fullsrc="imagenes/2.jpg" data-caption="Naturaleza Viva - Por [Artista 2]">
                <img src="imagenes/2.jpg" alt="Naturaleza Viva">
                <h3>Naturaleza Viva</h3>
                <p>Una explosión de colores que celebra la biodiversidad de nuestra región.</p>
                <div class="vote-info">
                    <button class="vote-button" data-cuadro-id="2">Votar</button>
                    <span class="vote-count" id="votes-2">0 votos</span>
                </div>
            </div>

            <!-- Cuadro 3 -->
            <div class="cuadro-item" data-fullsrc="imagenes/3.jpg" data-caption="Rostros de Nuestra Gente - Por [Artista 3]">
                <img src="imagenes/3.jpg" alt="Rostros de Nuestra Gente">
                <h3>Rostros de Nuestra Gente</h3>
                <p>Retratos que reflejan la riqueza cultural y las historias de nuestra comunidad.</p>
                <div class="vote-info">
                    <button class="vote-button" data-cuadro-id="3">Votar</button>
                    <span class="vote-count" id="votes-3">0 votos</span>
                </div>
            </div>

            <!-- Cuadro 4 -->
            <div class="cuadro-item" data-fullsrc="imagenes/4.jpg" data-caption="Abstracto Urbano - Por [Artista 4]">
                <img src="imagenes/4.jpg" alt="Abstracto Urbano">
                <h3>Abstracto Urbano</h3>
                <p>Una interpretación moderna de la energía y el dinamismo de la ciudad.</p>
                <div class="vote-info">
                    <button class="vote-button" data-cuadro-id="4">Votar</button>
                    <span class="vote-count" id="votes-4">0 votos</span>
                </div>
            </div>

            <!-- Cuadro 5 -->
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

    <!-- El Modal de Previsualización -->
    <div id="myModal" class="modal">
        <span class="close">&times;</span>
        <div class="modal-content">
            <img class="modal-img" id="img01" src="">
            <div id="caption" class="modal-caption"></div>
        </div>
    </div>

    <script>
        // Objeto para almacenar los votos. Se inicializa con 0 o con los datos de localStorage.
        let votes = JSON.parse(localStorage.getItem('cuadroVotes')) || {
            1: 0,
            2: 0,
            3: 0,
            4: 0,
            5: 0
        };

        // Bandera que indica si el usuario ya votó alguna vez (persiste en localStorage)
        let userHasVoted = localStorage.getItem('userHasVoted') === 'true'; // Convertir a booleano

        // Función para actualizar la visualización de los votos en la página
        function updateVoteCounts() {
            for (const cuadroId in votes) {
                const voteCountSpan = document.getElementById(`votes-${cuadroId}`);
                if (voteCountSpan) {
                    voteCountSpan.textContent = `${votes[cuadroId]} votos`;
                }
            }
        }

        // Función para deshabilitar todos los botones de votar
        function disableAllVoteButtons() {
            const voteButtons = document.getElementsByClassName("vote-button");
            for (let i = 0; i < voteButtons.length; i++) {
                voteButtons[i].classList.add("disabled");
                voteButtons[i].setAttribute("disabled", "true");
            }
        }

        // Se llama al cargar la página para mostrar los votos guardados y deshabilitar si ya votó
        document.addEventListener('DOMContentLoaded', () => {
            updateVoteCounts(); // Muestra los votos actuales
            if (userHasVoted) { // Si ya votó, deshabilita los botones al cargar la página
                disableAllVoteButtons();
                alert("Ya has emitido tu voto. ¡Gracias por participar!");
            }
        });

        // Lógica para el Modal de Previsualización (sin cambios)
        var modal = document.getElementById("myModal");
        var modalImg = document.getElementById("img01");
        var captionText = document.getElementById("caption");
        var cuadros = document.getElementsByClassName("cuadro-item");

        for (var i = 0; i < cuadros.length; i++) {
            var cuadro = cuadros[i];
            cuadro.querySelector("img").onclick = function(){
                modal.style.display = "flex";
                modalImg.src = this.parentElement.getAttribute("data-fullsrc");
                captionText.innerHTML = this.parentElement.getAttribute("data-caption");
            }
        }

        // Cierra el modal al hacer clic en la "x"
        var span = document.getElementsByClassName("close")[0];
        span.onclick = function() {
          modal.style.display = "none";
        }

        // Cierra el modal al hacer clic fuera de la imagen (en el fondo oscuro)
        modal.onclick = function(event) {
            if (event.target == modal) {
                modal.style.display = "none";
            }
        }

        // Lógica de Votación
        var voteButtons = document.getElementsByClassName("vote-button");
        for (var i = 0; i < voteButtons.length; i++) {
            voteButtons[i].onclick = function() {
                // Si el usuario ya votó, no permitir otro voto y avisar.
                if (userHasVoted) {
                    alert("Ya has emitido tu voto. No puedes votar de nuevo.");
                    return; // Sale de la función sin procesar el voto
                }

                var cuadroId = this.getAttribute("data-cuadro-id");

                // Incrementar el contador de votos
                votes[cuadroId]++;

                // Guardar los votos en localStorage
                localStorage.setItem('cuadroVotes', JSON.stringify(votes));

                // Actualizar la visualización de los votos
                updateVoteCounts();

                // Marcar que el usuario ya votó
                userHasVoted = true;
                localStorage.setItem('userHasVoted', 'true'); // Guardar en localStorage

                // Deshabilitar todos los botones de votar
                disableAllVoteButtons();

                alert("¡Voto registrado por el Cuadro " + cuadroId + "! ¡Gracias por participar!");
            }
        }
    </script>
</body>
</html>
