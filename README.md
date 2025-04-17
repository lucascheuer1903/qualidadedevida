# qualidadedevida
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Melhore sua Qualidade de Vida</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/tsparticles@2/tsparticles.bundle.min.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=Rubik:wght@400;600&display=swap" rel="stylesheet">
  <style>
    * {
      box-sizing: border-box;
    }

    body, html {
      margin: 0;
      padding: 0;
      font-family: 'Rubik', sans-serif;
      background: linear-gradient(135deg, #000000, #ffffff); /* Degradê preto e branco */
      color: #f0f0f0;
      overflow-x: hidden;
    }

    #particles-js {
      position: fixed;
      width: 100%;
      height: 100%;
      z-index: -1;
    }

    .container {
      padding: 30px 20px;
      max-width: 900px;
      margin: auto;
      text-align: center;
    }

    h1, h2 {
      color: #00ff99;
      margin-bottom: 10px;
    }

    .input-container {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 10px;
      margin: 20px 0;
    }

    .input-container input {
      padding: 12px;
      font-size: 16px;
      width: 160px;
      border-radius: 8px;
      border: none;
      text-align: center;
      background: #1e1e1e;
      color: #f0f0f0;
    }

    button {
      padding: 12px 24px;
      font-size: 18px;
      background: #00b894;
      color: white;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      transition: transform 0.2s ease, background 0.3s ease;
    }

    button:hover {
      background: #55efc4;
      transform: scale(1.05);
    }

    .dica {
      background: #1e1e1e;
      padding: 15px;
      margin: 10px 0;
      border-left: 5px solid #00ff99;
      border-radius: 5px;
      text-align: left;
    }

    .alerta {
      color: #ff7675;
      font-weight: bold;
      margin-top: 10px;
    }

    .galeria {
      margin: 40px auto;
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 15px;
    }

    .galeria img {
      width: 250px;
      height: 160px;
      object-fit: cover;
      border-radius: 10px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.5);
      transition: transform 0.3s ease;
    }

    .galeria img:hover {
      transform: scale(1.05);
    }

    #graficoContainer {
      margin-top: 40px;
      max-width: 500px;
      margin-left: auto;
      margin-right: auto;
    }

    .turismo {
      margin-top: 50px;
      text-align: left;
    }

    .assinatura {
      margin-top: 40px;
      font-size: 18px;
      color: #ffffff;
      background: rgba(0, 0, 0, 0.7); /* Fundo escuro para destacar */
      padding: 20px;
      border-radius: 8px;
      text-align: center;
      box-shadow: 0 4px 10px rgba(0,0,0,0.5);
    }

    /* Responsividade para celular */
    @media (max-width: 768px) {
      .input-container {
        flex-direction: column;
        gap: 15px;
      }

      .input-container input {
        width: 100%;
      }

      .container {
        padding: 20px;
      }

      h1 {
        font-size: 24px;
      }

      h2 {
        font-size: 20px;
      }

      .galeria img {
        width: 200px;
        height: 130px;
      }

      .turismo ul {
        padding-left: 20px;
      }

      .assinatura {
        font-size: 16px;
      }
    }
  </style>
</head>
<body>
  <div id="particles-js"></div>
  <div class="container">
    <h1>Como está sua qualidade de vida?</h1>
    <p>Preencha seus dados abaixo:</p>

    <div class="input-container">
      <input type="text" id="nome" placeholder="Seu nome" />
      <input type="date" id="data" />
    </div>

    <p>Insira o tempo que você passa no celular por dia:</p>
    <div class="input-container">
      <input type="text" id="tempo" placeholder="Ex: 2h30, 45m, 5h" />
      <button onclick="mostrarDicas()">Ver dicas</button>
    </div>

    <div id="dicas"></div>

    <div id="graficoContainer">
      <canvas id="graficoUso"></canvas>
    </div>

    <h2>💪 Inspire-se!</h2>
    <div class="galeria">
      <img src="https://images.pexels.com/photos/3757374/pexels-photo-3757374.jpeg?auto=compress&cs=tinysrgb&w=600" alt="Corrida">
      <img src="https://images.pexels.com/photos/949129/pexels-photo-949129.jpeg?auto=compress&cs=tinysrgb&w=600" alt="Musculação">
      <img src="https://images.pexels.com/photos/4761795/pexels-photo-4761795.jpeg?auto=compress&cs=tinysrgb&w=600" alt="Luta">
    </div>

    <div class="turismo">
      <h2>🌆 Pontos turísticos de Guaíba, RS</h2>
      <ul>
        <li><strong>Orla de Guaíba:</strong> Ótima para caminhadas e ver o pôr do sol.</li>
        <li><strong>Museu Carlos Nobre:</strong> História e cultura da cidade.</li>
        <li><strong>Igreja Nossa Senhora do Livramento:</strong> Arquitetura histórica.</li>
        <li><strong>Parque da Juventude:</strong> Para se exercitar ao ar livre.</li>
      </ul>
    </div>

    <div class="assinatura">
      Site desenvolvido por <strong>Lucas Scheurmann</strong> e <strong>Letícia Freitas</strong> da turma <strong>302</strong>.
    </div>
  </div>

  <script>
    let grafico;

    function mostrarDicas() {
      const nome = document.getElementById("nome").value;
      const data = document.getElementById("data").value;
      const tempoStr = document.getElementById('tempo').value.trim().toLowerCase();
      const dicasContainer = document.getElementById('dicas');
      const ctx = document.getElementById('graficoUso').getContext('2d');
      dicasContainer.innerHTML = '';

      const regex = /(?:(\d+)\s*h)?\s*(?:(\d+)\s*m)?/;
      const match = tempoStr.match(regex);

      const horas = parseInt(match[1]) || 0;
      const minutos = parseInt(match[2]) || 0;
      const totalHoras = horas + minutos / 60;

      if (!nome || !data || totalHoras <= 0 || totalHoras > 24) {
        dicasContainer.innerHTML = '<p>Preencha todos os campos corretamente (nome, data e tempo).</p>';
        return;
      }

      dicasContainer.innerHTML = `<h3>Olá, ${nome}! Aqui estão suas dicas para o dia ${data}:</h3>`;

      let dicas = [];

      if (totalHoras <= 2) {
        dicas = [
          "Você já tem um bom controle! Continue aproveitando seu tempo longe das telas.",
          "Inclua pequenas caminhadas no seu dia, mesmo que sejam de 10 minutos.",
        ];
      } else if (totalHoras <= 5) {
        dicas = [
          "Que tal deixar o celular de lado por uma hora e ler um livro?",
          "Experimente atividades como meditação ou exercícios leves.",
        ];
      } else if (totalHoras <= 8) {
        dicas = [
          "Tente fazer pausas a cada hora de uso de tela.",
          "Agende um tempo para hobbies offline: pintar, tocar instrumento, cozinhar.",
        ];
      } else {
        dicas = [
          "Considere um 'desafio sem celular' por 1 dia na semana.",
          "Converse pessoalmente com amigos ou familiares.",
          "Faça uma lista de atividades que você gostaria de fazer fora das telas.",
        ];

        const alerta = document.createElement("p");
        alerta.className = "alerta";
        alerta.textContent = "⚠️ Cuidado! Seu tempo de uso está acima do ideal.";
        dicasContainer.appendChild(alerta);
      }

      dicas.forEach(dica => {
        const div = document.createElement('div');
        div.className = 'dica';
        div.textContent = dica;
        dicasContainer.appendChild(div);
      });

      if (grafico) grafico.destroy();

      grafico = new Chart(ctx, {
        type: 'doughnut',
        data: {
          labels: ['Tempo no celular', 'Tempo livre'],
          datasets: [{
            data: [totalHoras, 24 - totalHoras],
            backgroundColor: ['#00ff99', '#2d3436'],
            borderColor: '#000',
            borderWidth: 2
          }]
        },
        options: {
          plugins: {
            legend: {
              labels: {
                color: '#fff'
              }
            }
          }
        }
      });
    }

    // Partículas de fundo
    tsParticles.load("particles-js", {
      fullScreen: { enable: false },
      background: {
        color: "#0a0a0a"
      },
      particles: {
        color: { value: "#00ff99" },
        links: { enable: true, color: "#00ff99" },
        move: { enable: true, speed: 1 },
        number: { value: 60 },
        size: { value: 3 },
      },
    });
  </script>
</body>
</html>
