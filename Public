<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <title>¿Con Quién? Online</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f2f2f2;
      text-align: center;
      margin: 0; padding: 20px;
    }
    #cartas {
      margin-top: 20px;
    }
    .carta {
      display: inline-block;
      border: 2px solid #000;
      padding: 10px;
      margin: 5px;
      width: 50px;
      height: 70px;
      background-color: #fff;
      border-radius: 8px;
      font-size: 18px;
      font-weight: bold;
      user-select: none;
    }
    .roja { color: red; }
    .negra { color: black; }
    #status {
      margin-top: 20px;
      font-weight: bold;
      font-size: 18px;
    }
    #botonConQuien {
      margin-top: 20px;
      padding: 10px 20px;
      font-size: 18px;
      background-color: #4caf50;
      color: white;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      display: none;
    }
  </style>
</head>
<body>
  <h1>¿Con Quién? Online</h1>
  <div>
    <label>Nombre: <input type="text" id="playerName" /></label>
    <label>Sala: <input type="text" id="roomId" /></label>
    <button id="joinBtn">Entrar a la sala</button>
  </div>

  <div id="status"></div>
  <div id="cartas"></div>
  <button id="botonConQuien">¡Con Quién!</button>

  <script src="/socket.io/socket.io.js"></script>
  <script>
    const socket = io();
    const joinBtn = document.getElementById('joinBtn');
    const playerNameInput = document.getElementById('playerName');
    const roomIdInput = document.getElementById('roomId');
    const statusDiv = document.getElementById('status');
    const cartasDiv = document.getElementById('cartas');
    const botonConQuien = document.getElementById('botonConQuien');

    let playerName = '';
    let roomId = '';

    joinBtn.onclick = () => {
      playerName = playerNameInput.value.trim();
      roomId = roomIdInput.value.trim();

      if (!playerName || !roomId) {
        alert('Por favor ingresa nombre y sala.');
        return;
      }

      socket.emit('joinRoom', { playerName, roomId });
      statusDiv.textContent = 'Esperando jugadores en la sala...';
    };

    socket.on('repartirCartas', (mano) => {
      statusDiv.textContent = 'Juego iniciado. Tus cartas:';
      cartasDiv.innerHTML = '';
      mano.forEach(carta => {
        const div = document.createElement('div');
        div.className = 'carta ' + (carta.includes('♥') || carta.includes('♦') ? 'roja' : 'negra');
        div.textContent = carta;
        cartasDiv.appendChild(div);
      });
      botonConQuien.style.display = 'inline-block';
    });

    botonConQuien.onclick = () => {
      socket.emit('conQuien', roomId);
      alert('¡Con Quién! Has ganado o estás listo.');
    };

    socket.on('alguienGano', (idGanador) => {
      if (socket.id !== idGanador) {
        alert('Otro jugador dijo ¡Con Quién!');
      }
    });
  </script>
</body>
</html>
