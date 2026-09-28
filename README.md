# Escalonesgalacticos-
Salta cad vez que toques el botón
<div id="game4" style="width:100%;max-width:520px;margin:auto;font-family:system-ui,sans-serif">

  <style>
    #game4 .scene {
      position: relative;
      height: 520px;
      overflow: hidden;
      border-radius: 18px;
      background: linear-gradient(#18243a,#080d18);
      touch-action: manipulation;
    }

    #game4 .world {
      position: absolute;
      left: 0;
      top: 0;
      width: 100%;
      height: 100%;
      will-change: transform;
    }

    #game4 .step {
      position: absolute;
      height: 18px;
      border-radius: 9px;
      background: #58d66d;
      box-shadow: 0 4px 0 #277c38;
    }

    #game4 .player {
      position: absolute;
      z-index: 30;
      width: 40px;
      height: 54px;
      border-radius: 18px 18px 12px 12px;
      background: #ffd34d;
      box-shadow: inset 0 -8px #e5a92f;
      will-change: top;
    }

    #game4 .eye {
      position: absolute;
      top: 13px;
      width: 6px;
      height: 6px;
      border-radius: 50%;
      background: #18243a;
    }

    #game4 .eye1 {
      left: 9px;
    }

    #game4 .eye2 {
      right: 9px;
    }

    #game4 .score {
      position: absolute;
      z-index: 50;
      top: 14px;
      left: 16px;
      color: #fff;
      font-size: 22px;
      font-weight: 800;
      text-shadow: 0 2px 4px #000;
    }

    #game4 button {
      width: 100%;
      height: 54px;
      margin-top: 12px;
      border: 0;
      border-radius: 14px;
      background: #5969ff;
      color: white;
      font-size: 19px;
      font-weight: 800;
    }

    #game4 .hint {
      text-align: center;
      color: #aab4c7;
      font-size: 13px;
      margin-top: 7px;
    }
  </style>


  <div class="scene" id="scene4">

    <div class="score" id="score4">
      Puntos: 0
    </div>

    <div class="world" id="world4"></div>

    <div class="player" id="player4">
      <span class="eye eye1"></span>
      <span class="eye eye2"></span>
    </div>

  </div>


  <button id="jump4" type="button">
    SALTAR
  </button>

  <div class="hint">
    Toca SALTAR o la pantalla
  </div>


  <script>

    (() => {

      const scene =
        document.getElementById("scene4");

      const world =
        document.getElementById("world4");

      const player =
        document.getElementById("player4");

      const scoreText =
        document.getElementById("score4");

      const btn =
        document.getElementById("jump4");


      const STEP = 75;

      let score = 0;
      let jumping = false;

      let worldY = 0;
      let cameraY = 0;


      // Crear escaleras
      for (let i = 0; i < 100; i++) {

        const s = document.createElement("div");

        s.className = "step";

        const side =
          i % 2 === 0 ? -30 : 30;

        s.style.left =
          `calc(50% - 60px + ${side}px)`;

        s.style.top =
          (-i * STEP) + "px";

        s.style.width = "120px";

        world.appendChild(s);
      }


      // Dibujar jugador + cámara
      function draw() {

        const height =
          scene.clientHeight;

        // Lugar donde queremos mantener
        // al muñeco en pantalla
        const playerScreenY =
          height * 0.48;


        // La cámara sigue al muñeco
        cameraY =
          playerScreenY -
          worldY -
          27;


        // Mover el mundo
        world.style.transform =
          `translateY(${height - 35 + cameraY}px)`;


        // Mantener al muñeco visible
        player.style.left =
          (scene.clientWidth / 2) + "px";

        player.style.top =
          playerScreenY + "px";

        player.style.transform =
          "translateX(-50%)";
      }


      // Saltar
      function jump() {

        if (jumping) {
          return;
        }

        jumping = true;

        score++;

        scoreText.textContent =
          "Puntos: " + score;


        const start =
          performance.now();

        const duration = 520;


        const from =
          -(score - 1) * STEP;

        const to =
          -score * STEP;


        function frame(now) {

          let progress =
            (now - start) / duration;


          if (progress > 1) {
            progress = 1;
          }


          // Movimiento suave
          const smooth =
            progress * progress *
            (3 - 2 * progress);


          // Movimiento vertical del muñeco
          const jumpHeight =
            Math.sin(progress * Math.PI) * 60;


          worldY =
            from +
            (to - from) * smooth -
            jumpHeight;


          // Actualizar cámara
          draw();


          if (progress < 1) {

            requestAnimationFrame(frame);

          } else {

            worldY = to;

            draw();

            jumping = false;
          }
        }


        requestAnimationFrame(frame);
      }


      // Botón
      btn.addEventListener(
        "click",
        jump
      );


      // Tocar pantalla
      scene.addEventListener(
        "pointerdown",
        (event) => {

          if (event.target !== btn) {
            jump();
          }

        }
      );


      // Si cambia el tamaño
      window.addEventListener(
        "resize",
        draw
      );


      // Posición inicial
      draw();

    })();

  </script>

</div> 
