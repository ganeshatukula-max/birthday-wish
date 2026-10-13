# birthday-wish
for pursonal purpose

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Happy Birthday! 🎂</title>

  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      min-height: 100vh;
      min-height: 100dvh;
      display: flex;
      align-items: center;
      justify-content: center;
      overflow-x: hidden;
      font-family: "Trebuchet MS", Arial, sans-serif;
      color: white;
      text-align: center;
      background: linear-gradient(
        135deg, #170b38, #591b76, #bd367e, #ff8c69
      );
      background-size: 300% 300%;
      animation: gradient 10s ease infinite;
      padding: 24px 14px;
    }

    @keyframes gradient {
      0%, 100% { background-position: 0% 50%; }
      50% { background-position: 100% 50%; }
    }

    .card {
      width: 100%;
      max-width: 540px;
      padding: 35px 22px;
      border-radius: 28px;
      background: rgba(255, 255, 255, 0.12);
      border: 1px solid rgba(255, 255, 255, 0.28);
      backdrop-filter: blur(14px);
      -webkit-backdrop-filter: blur(14px);
      box-shadow: 0 20px 60px rgba(0, 0, 0, 0.25);
      animation: appear 1s ease-out;
      position: relative;
      z-index: 1;
    }

    @keyframes appear {
      from { opacity: 0; transform: translateY(30px) scale(.95); }
      to { opacity: 1; transform: translateY(0) scale(1); }
    }

    .top-emoji {
      font-size: clamp(38px, 10vw, 58px);
      animation: bounce 2s ease-in-out infinite;
    }

    @keyframes bounce {
      0%, 100% { transform: translateY(0) rotate(-5deg); }
      50% { transform: translateY(-12px) rotate(5deg); }
    }

    h1 {
      margin: 15px 0;
      font-size: clamp(2rem, 8vw, 3.5rem);
      line-height: 1.15;
      color: #fff4a3;
      text-shadow: 0 0 18px rgba(255, 220, 100, .65);
    }

    .name {
      font-size: clamp(1.7rem, 7vw, 2.6rem);
      font-weight: bold;
      color: #ffcfef;
      margin: 14px 0;
      overflow-wrap: anywhere;
    }

    .cake {
      font-size: clamp(70px, 20vw, 105px);
      margin: 12px 0;
      filter: drop-shadow(0 0 18px rgba(255, 210, 100, .4));
      animation: cakeFloat 3s ease-in-out infinite;
    }

    @keyframes cakeFloat {
      0%, 100% { transform: translateY(0); }
      50% { transform: translateY(-8px); }
    }

    .message {
      font-size: clamp(15px, 4vw, 18px);
      line-height: 1.8;
      color: #fff8ff;
      margin: 15px auto 24px;
      max-width: 410px;
    }

    button {
      background: linear-gradient(135deg, #fff09b, #ffbd59);
      color: #591b55;
      border: none;
      border-radius: 50px;
      padding: 16px 25px;
      font-size: 16px;
      font-weight: bold;
      cursor: pointer;
      box-shadow: 0 8px 24px rgba(0, 0, 0, .2);
      transition: transform .2s, box-shadow .2s;
      max-width: 100%;
      touch-action: manipulation;
    }

    button:hover {
      transform: translateY(-3px) scale(1.03);
      box-shadow: 0 12px 30px rgba(255, 210, 100, .35);
    }

    button:active {
      transform: scale(.97);
    }

    button:focus-visible {
      outline: 3px solid white;
      outline-offset: 4px;
    }

    .footer {
      margin-top: 25px;
      font-size: 13px;
      color: rgba(255, 255, 255, .8);
    }

    .sparkle {
      position: fixed;
      top: -30px;
      pointer-events: none;
      z-index: 0;
      animation: fall linear forwards;
      will-change: transform;
    }

    @keyframes fall {
      to {
        transform: translate3d(var(--drift), 110vh, 0) rotate(720deg);
      }
    }

    #confetti-canvas {
      position: fixed;
      inset: 0;
      width: 100%;
      height: 100%;
      pointer-events: none;
      z-index: 5;
    }

    @media (max-width: 400px) {
      .card {
        padding: 28px 17px;
        border-radius: 22px;
      }

      button {
        width: 100%;
      }
    }

    @media (prefers-reduced-motion: reduce) {
      *, *::before, *::after {
        animation-duration: .01ms !important;
        animation-iteration-count: 1 !important;
        scroll-behavior: auto !important;
      }
    }
  </style>
</head>

<body>
  <canvas id="confetti-canvas" aria-hidden="true"></canvas>

  <main class="card">
    <div class="top-emoji" aria-hidden="true">🎉🎈🎁</div>

    <h1>Happy Birthday!</h1>

    <!-- Change the name below -->
    <div class="name" id="birthdayName">Your Name 💖</div>

    <div class="cake" aria-hidden="true">🎂</div>

    <p class="message">
      May your special day be filled with endless happiness,
      beautiful surprises, lots of laughter, and unforgettable
      memories. May all your dreams come true and your smile
      shine brighter every day! ✨
      <br><br>
      You deserve all the love and magic in the world. 💕
    </p>

    <button id="celebrateBtn" type="button">
      🎊 Click for a Surprise! 🎊
    </button>

    <p class="footer">
      Made with ❤️ just for you
    </p>
  </main>

  <script>
    // Customize the birthday person's name here.
    const birthdayPerson = "Your Name";
    document.getElementById("birthdayName").textContent =
      birthdayPerson + " 💖";

    // Lightweight canvas confetti animation.
    const canvas = document.getElementById("confetti-canvas");
    const ctx = canvas.getContext("2d");
    const colors = [
      "#ff4d8d", "#ffe66d", "#7cf7d4",
      "#a78bfa", "#ffffff", "#ff9f43"
    ];

    let particles = [];
    let animationId = null;
    let width = 0;
    let height = 0;

    function resizeCanvas() {
      const dpr = Math.min(window.devicePixelRatio || 1, 2);
      width = window.innerWidth;
      height = window.innerHeight;
      canvas.width = width * dpr;
      canvas.height = height * dpr;
      ctx.setTransform(dpr, 0, 0, dpr, 0, 0);
    }

    window.addEventListener("resize", resizeCanvas);
    resizeCanvas();

    function burst(count = 180) {
      const originX = width / 2;
      const originY = height * 0.4;

      for (let i = 0; i < count; i++) {
        const angle = Math.random() * Math.PI * 2;
        const speed = 2 + Math.random() * 7;

        particles.push({
          x: originX,
          y: originY,
          vx: Math.cos(angle) * speed,
          vy: Math.sin(angle) * speed - 2,
          gravity: 0.10 + Math.random() * 0.08,
          size: 3 + Math.random() * 6,
          color: colors[Math.floor(Math.random() * colors.length)],
          rotation: Math.random() * 360,
          spin: (Math.random() - 0.5) * 12,
          life: 100 + Math.random() * 65
        });
      }

      // Keep the animation lightweight on smaller devices.
      particles = particles.slice(-650);

      if (!animationId) {
        animate();
      }
    }

    function animate() {
      ctx.clearRect(0, 0, width, height);

      particles = particles.filter(p => p.life > 0);

      for (const p of particles) {
        p.x += p.vx;
        p.y += p.vy;
        p.vy += p.gravity;
        p.vx *= 0.995;
        p.rotation += p.spin;
        p.life--;

        ctx.save();
        ctx.translate(p.x, p.y);
        ctx.rotate(p.rotation * Math.PI / 180);
        ctx.globalAlpha = Math.min(1, p.life / 25);
        ctx.fillStyle = p.color;
        ctx.fillRect(-p.size / 2, -p.size / 2, p.size, p.size * 0.65);
        ctx.restore();
      }

      if (particles.length) {
        animationId = requestAnimationFrame(animate);
      } else {
        ctx.clearRect(0, 0, width, height);
        animationId = null;
      }
    }

    // Gentle falling sparkles in the background.
    const reducedMotion = window.matchMedia(
      "(prefers-reduced-motion: reduce)"
    ).matches;

    function createSparkle() {
      if (reducedMotion || document.hidden) return;

      const sparkle = document.createElement("div");
      sparkle.className = "sparkle";
      sparkle.textContent = ["✨", "💖", "⭐", "🎊"][
        Math.floor(Math.random() * 4)
      ];

      sparkle.style.left = Math.random() * 100 + "vw";
      sparkle.style.setProperty(
        "--drift", (Math.random() * 140 - 70) + "px"
      );
      sparkle.style.fontSize = (12 + Math.random() * 18) + "px";
      sparkle.style.animationDuration = (5 + Math.random() * 5) + "s";

      document.body.appendChild(sparkle);
      sparkle.addEventListener("animationend", () => sparkle.remove());
    }

    if (!reducedMotion) {
      setInterval(createSparkle, 650);
    }

    document.getElementById("celebrateBtn").addEventListener("click", () => {
      burst(260);

      const button = document.getElementById("celebrateBtn");
      button.textContent = "🎉 Happy Birthday! 🎉";

      setTimeout(() => {
        button.textContent = "🎊 Celebrate Again! 🎊";
      }, 1800);
    });

    // Welcome celebration when the website first opens.
    if (!reducedMotion) {
      setTimeout(() => burst(150), 500);
    }
  </script>
</body>
</html>
