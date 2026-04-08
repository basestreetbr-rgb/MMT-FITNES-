<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MMT Fitness | Centro de Treinamento</title>
    
    <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;700;900&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

    <style>
        :root {
            --bg-fosco: #121212;
            --vermelho: #ff0000;
            --vermelho-dark: #8b0000;
            --branco: #ffffff;
            --cinza: #1a1a1a;
            --transp-red: rgba(255, 0, 0, 0.15);
        }

        * { margin: 0; padding: 0; box-sizing: border-box; }

        body {
            background-color: var(--bg-fosco);
            color: var(--branco);
            font-family: 'Montserrat', sans-serif;
            overflow-x: hidden;
            scroll-behavior: smooth;
            line-height: 1.6;
        }

        /* Canvas Background */
        #hex-canvas { 
            position: fixed; 
            top: 0; 
            left: 0; 
            z-index: -1; 
            pointer-events: none;
            opacity: 0.3;
        }

        /* Loader */
        #loader {
            position: fixed; width: 100%; height: 100vh; background: #000;
            display: flex; justify-content: center; align-items: center; z-index: 9999;
            transition: opacity 0.8s ease;
        }
        .logo-loader { font-size: 3rem; font-weight: 900; letter-spacing: 5px; animation: pulse 1.5s infinite; }
        @keyframes pulse { 0% { transform: scale(1); opacity: 1; } 50% { transform: scale(1.1); opacity: 0.5; } 100% { transform: scale(1); opacity: 1; } }

        /* Header */
        header {
            padding: 20px 5%; display: flex; justify-content: space-between;
            align-items: center; position: absolute; width: 100%; z-index: 100;
        }
        .logo-circle {
            width: 70px; height: 70px; border: 4px solid var(--vermelho);
            border-radius: 50%; display: flex; align-items: center; justify-content: center;
            background: rgba(0,0,0,0.8);
        }
        .logo-text { font-weight: 900; font-size: 1.2rem; }

        /* Hero */
        .hero { height: 100vh; display: flex; flex-direction: column; justify-content: center; padding: 0 10%; }
        .red-line { width: 100px; height: 6px; background: var(--vermelho); margin-bottom: 20px; }
        h1 { font-size: clamp(2.5rem, 8vw, 5rem); font-weight: 900; line-height: 1; text-transform: uppercase; }
        .text-red { color: var(--vermelho); }

        /* Seções Gerais */
        section { padding: 80px 10%; position: relative; z-index: 1; }
        .section-title { text-align: center; font-size: clamp(2rem, 5vw, 3rem); font-weight: 900; margin-bottom: 50px; text-transform: uppercase; }

        /* Diferenciais */
        .benefits-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); gap: 30px; }
        .benefit-card {
            background: rgba(26, 26, 26, 0.9); padding: 40px; border-radius: 8px;
            border-left: 3px solid var(--transp-red); transition: 0.3s; text-align: center;
        }
        .benefit-card:hover { border-left: 3px solid var(--vermelho); transform: translateY(-5px); }
        .benefit-icon { font-size: 3rem; color: var(--vermelho); margin-bottom: 25px; }

        /* Tabelas e Horários */
        .table-container { 
            overflow-x: auto; 
            background: var(--cinza); 
            padding: 20px; 
            border-radius: 10px; 
            border-bottom: 4px solid var(--vermelho);
            margin-bottom: 40px;
        }
        table { width: 100%; border-collapse: collapse; min-width: 600px; }
        th { background: var(--vermelho-dark); color: white; padding: 15px; font-size: 0.8rem; text-transform: uppercase; }
        td { padding: 15px; border-bottom: 1px solid #333; text-align: center; font-size: 0.9rem; }

        /* Abas de Preço */
        .tabs { display: flex; justify-content: center; gap: 10px; margin-bottom: 30px; flex-wrap: wrap; }
        .tab-btn { 
            padding: 12px 25px; background: transparent; border: 1px solid var(--vermelho); 
            color: white; cursor: pointer; font-weight: bold; text-transform: uppercase; transition: 0.3s;
        }
        .tab-btn.active { background: var(--vermelho); }
        .price-content { display: none; }
        .price-content.active { display: block; animation: fadeIn 0.5s ease; }

        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

        /* Avaliações */
        .reviews-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); gap: 25px; }
        .review-card { background: #1a1a1a; padding: 30px; border-radius: 8px; border-bottom: 3px solid var(--vermelho); }
        .stars { color: #ffcc00; margin-bottom: 10px; }

        /* Botões */
        .cta-btn {
            display: inline-block; padding: 16px 40px; background: var(--vermelho);
            color: white; text-decoration: none; font-weight: bold; border-radius: 4px;
            text-transform: uppercase; transition: 0.3s; cursor: pointer; border: none;
        }
        .cta-btn:hover { box-shadow: 0 0 25px var(--vermelho); transform: scale(1.05); }

        /* Footer */
        footer { padding: 60px 10%; text-align: center; border-top: 1px solid var(--transp-red); background: rgba(0,0,0,0.8); }
        .social-icons a { color: white; font-size: 2rem; margin: 0 15px; transition: 0.3s; display: inline-block; }
        .social-icons a:hover { color: var(--vermelho); transform: translateY(-5px); }

        @media (max-width: 768px) { section { padding: 60px 5%; } .hero { padding: 0 5%; } }
    </style>
</head>
<body>

    <div id="loader">
        <div class="logo-loader">MMT</div>
    </div>

    <canvas id="hex-canvas"></canvas>

    <header>
        <div class="logo-circle"><span class="logo-text">MMT</span></div>
        <button onclick="shareSite()" style="background:none; border:1px solid #fff; color:#fff; padding:10px 20px; border-radius:20px; cursor:pointer;">
            <i class="fas fa-share-alt"></i>
        </button>
    </header>

    <main>
        <section class="hero">
            <div class="red-line"></div>
            <h1>A MAIOR <br><span class="text-red">PERFORMANCE</span></h1>
            <p>R. Emancipação, 1050, Picada Café - RS</p>
            <a href="https://wa.me/5554991904826" class="cta-btn"><i class="fab fa-whatsapp"></i> Matricule-se</a>
        </section>

        <section id="diferenciais">
            <h2 class="section-title">Por que treinar <span class="text-red">conosco?</span></h2>
            <div class="benefits-grid">
                <div class="benefit-card">
                    <i class="fas fa-heartbeat benefit-icon"></i>
                    <h3>Programa 60+</h3>
                    <p>Treinamento personalizado focado em saúde e mobilidade para todas as idades.</p>
                </div>
                <div class="benefit-card">
                    <i class="fas fa-user-check benefit-icon"></i>
                    <h3>Qualificação</h3>
                    <p>Profissionais graduados na área da saúde e atendimento de excelência.</p>
                </div>
                <div class="benefit-card">
                    <i class="fas fa-dumbbell benefit-icon"></i>
                    <h3>Foco no Resultado</h3>
                    <p>Equipamentos modernos e ambiente planejado para quem busca a perfeição.</p>
                </div>
            </div>
        </section>

        <section id="horarios">
            <h2 class="section-title">Nossos <span class="text-red">Horários</span></h2>
            <div class="table-container">
                <div style="text-align: center; margin-bottom: 30px;">
                    <h3 class="text-red">ACADEMIA ABERTA</h3>
                    <p>Segunda a Quinta: 5h30 às 21h30 | Sexta: 5h30 às 21h | Sábado: 7h30 às 11h30</p>
                </div>
                <h3 style="text-align: center; margin-bottom: 15px;">TREINAMENTO FUNCIONAL</h3>
                <table>
                    <thead>
                        <tr><th>Horário</th><th>SEG</th><th>TER</th><th>QUA</th><th>QUI</th><th>SEX</th></tr>
                    </thead>
                    <tbody>
                        <tr><td>06h</td><td>-</td><td>Funcional</td><td>-</td><td>Funcional</td><td>-</td></tr>
                        <tr><td>08h</td><td>Funcional</td><td>-</td><td>Funcional</td><td>-</td><td>Funcional</td></tr>
                        <tr><td>18h</td><td>Funcional</td><td>Funcional</td><td>Funcional</td><td>Funcional</td><td>Funcional</td></tr>
                        <tr><td>19h</td><td>Funcional</td><td>Funcional</td><td>Funcional</td><td>Funcional</td><td>-</td></tr>
                    </tbody>
                </table>
            </div>
        </section>

        <section id="valores">
            <h2 class="section-title">Planos e <span class="text-red">Valores</span></h2>
            <div class="tabs">
                <button class="tab-btn active" onclick="openTab(event, 'musc')">Musculação</button>
                <button class="tab-btn" onclick="openTab(event, 'func')">Funcional</button>
                <button class="tab-btn" onclick="openTab(event, 'comb')">Combo M+F</button>
            </div>

            <div id="musc" class="price-content active">
                <div class="table-container">
                    <table>
                        <thead><tr><th>Frequência</th><th>Mensal</th><th>Semestral</th><th>Anual</th></tr></thead>
                        <tbody>
                            <tr><td>Diária</td><td>R$ 50,00</td><td>-</td><td>-</td></tr>
                            <tr><td>2x Semana</td><td>R$ 135,00</td><td>R$ 115,00</td><td>R$ 105,00</td></tr>
                            <tr><td>3x Semana</td><td>R$ 160,00</td><td>R$ 145,00</td><td>R$ 135,00</td></tr>
                            <tr><td>Livre</td><td>R$ 175,00</td><td>R$ 160,00</td><td>R$ 145,00</td></tr>
                        </tbody>
                    </table>
                </div>
            </div>

            <div id="func" class="price-content">
                <div class="table-container">
                    <table>
                        <thead><tr><th>Frequência</th><th>Mensal</th><th>Semestral Cartão</th><th>Recorrência</th></tr></thead>
                        <tbody>
                            <tr><td>2x Semana</td><td>R$ 150,00</td><td>R$ 140,00</td><td>R$ 145,00</td></tr>
                            <tr><td>3x Semana</td><td>R$ 170,00</td><td>R$ 150,00</td><td>R$ 155,00</td></tr>
                            <tr><td>Livre</td><td>R$ 180,00</td><td>R$ 165,00</td><td>R$ 170,00</td></tr>
                        </tbody>
                    </table>
                </div>
            </div>

            <div id="comb" class="price-content">
                <div class="table-container">
                    <table>
                        <thead><tr><th>Modalidade</th><th>Mensal</th><th>Semestral</th><th>Recorrência</th></tr></thead>
                        <tbody>
                            <tr><td>Combo 1+1</td><td>R$ 155,00</td><td>R$ 145,00</td><td>R$ 150,00</td></tr>
                            <tr><td>Combo 2+1</td><td>R$ 175,00</td><td>R$ 155,00</td><td>R$ 160,00</td></tr>
                            <tr><td>Livre Total</td><td>R$ 200,00</td><td>R$ 190,00</td><td>R$ 190,00</td></tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </section>

        <section class="reviews-section">
            <h2 class="section-title">O que dizem os <span class="text-red">Atletas</span></h2>
            <div class="reviews-grid">
                <div class="review-card">
                    <div class="stars">★★★★★</div>
                    <p>"Ambiente nota dez e equipamentos de primeira. A melhor opção da região para quem busca treino sério."</p>
                    <div class="reviewer-name"><strong>Ricardo Mendes</strong></div>
                </div>

                <div class="review-card">
                    <div class="stars">★★★★★</div>
                    <p>"A atenção dos instrutores é impecável. O programa 60+ trouxe uma qualidade de vida que eu não esperava."</p>
                    <div class="reviewer-name"><strong>Mariana Schabarum</strong></div>
                </div>

                <div class="review-card">
                    <div class="stars">★★★★★</div>
                    <p>"O treino funcional é dinâmico e muito desafiador. Os professores realmente sabem o que estão fazendo."</p>
                    <div class="reviewer-name"><strong>Lucas Oliveira</strong></div>
                </div>

                <div class="review-card">
                    <div class="stars">★★★★★</div>
                    <p>"Academia extremamente limpa, organizada e com uma energia incrível. Dá gosto de treinar aqui todos os dias."</p>
                    <div class="reviewer-name"><strong>Carla Silveira</strong></div>
                </div>

                <div class="review-card">
                    <div class="stars">★★★★★</div>
                    <p>"Melhor custo-benefício de Picada Café. O plano de recorrência facilita muito a vida. Recomendo demais!"</p>
                    <div class="reviewer-name"><strong>Fernando Souza</strong></div>
                </div>
            </div>
        </section>

        <section style="text-align: center; background: rgba(255,0,0,0.05);">
            <h2 class="section-title">ONDE <span class="text-red">ESTAMOS</span></h2>
            <p style="margin-bottom: 20px;">R. Emancipação, 1050, Picada Café - RS</p>
            <a href="https://maps.app.goo.gl/ChIJVRtTjf5MGZURm5bepIEfIXs" target="_blank" class="cta-btn" style="background: #333;">
                <i class="fas fa-map-marked-alt"></i> Abrir no Maps
            </a>
        </section>
    </main>

    <footer>
        <div class="social-icons">
            <a href="https://www.instagram.com/mmt_academia?igsh=OWhhOGZrY2t1YWZ4" target="_blank"><i class="fab fa-instagram"></i></a>
            <a href="https://wa.me/5554991904826" target="_blank"><i class="fab fa-whatsapp"></i></a>
            <a href="tel:54991904826"><i class="fas fa-phone"></i></a>
        </div>
        <p style="margin-top: 20px; opacity: 0.5; font-size: 0.7rem;">© 2026 MMT FITNESS - SUPERE LIMITES, ELEVE RESULTADOS</p>
    </footer>

    <script>
        // Loader
        window.addEventListener('load', () => {
            setTimeout(() => {
                document.getElementById('loader').style.opacity = '0';
                setTimeout(() => document.getElementById('loader').style.display = 'none', 800);
            }, 1000);
        });

        // Tabs de Preço
        function openTab(evt, tabName) {
            var i, content, buttons;
            content = document.getElementsByClassName("price-content");
            for (i = 0; i < content.length; i++) { content[i].classList.remove("active"); }
            buttons = document.getElementsByClassName("tab-btn");
            for (i = 0; i < buttons.length; i++) { buttons[i].classList.remove("active"); }
            document.getElementById(tabName).classList.add("active");
            evt.currentTarget.classList.add("active");
        }

        // Hex Canvas Engine
        const canvas = document.getElementById('hex-canvas');
        const ctx = canvas.getContext('2d');
        let scrollY = 0;
        let targetScrollY = 0;

        function init() {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
        }

        window.addEventListener('resize', init);
        window.addEventListener('scroll', () => { targetScrollY = window.pageYOffset; });
        init();

        function drawHex(x, y, size) {
            ctx.beginPath();
            for (let i = 0; i < 6; i++) {
                ctx.lineTo(x + size * Math.cos(i * Math.PI / 3), y + size * Math.sin(i * Math.PI / 3));
            }
            ctx.closePath();
            ctx.strokeStyle = 'rgba(255, 0, 0, 0.15)';
            ctx.stroke();
        }

        function render() {
            scrollY += (targetScrollY - scrollY) * 0.05;
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            const size = 55;
            const vertDist = size * 1.5;
            const horizDist = Math.sqrt(3) * size;
            const move = scrollY * 0.2;

            for (let row = -2; row < (canvas.height / vertDist) + 2; row++) {
                for (let col = -2; col < (canvas.width / horizDist) + 2; col++) {
                    let x = col * horizDist + (row % 2 !== 0 ? horizDist / 2 : 0);
                    let y = row * vertDist - (move % vertDist);
                    drawHex(x, y, size);
                }
            }
            requestAnimationFrame(render);
        }
        render();

        function shareSite() {
            if (navigator.share) {
                navigator.share({ title: 'MMT Fitness', url: window.location.href });
            } else { alert("Link copiado!"); }
        }
    </script>
</body>
</html>
