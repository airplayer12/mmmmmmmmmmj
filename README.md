<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Reproductor de Música</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="player">
        <h2 id="track-title">Título de la Canción</h2>
        <h3 id="track-artist">Artista</h3>
        <h4 id="track-album">Álbum</h4>
        
        <audio id="audio" controls></audio>
        
        <div class="controls">
            <input type="file" id="file-input" accept="audio/mp3" multiple>
            <button id="prev-btn">⏮️</button>
            <button id="play-btn">▶️</button>
            <button id="next-btn">⏭️</button>
        </div>
        
        <div class="status-bar">
            <span id="current-time">00:00</span> / <span id="duration">00:00</span>
        </div>
    </div>
    
    <script src="script.js"></script>
</body>
</html>
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Reproductor de Música</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="player">
        <h2 id="track-title">Título de la Canción</h2>
        <h3 id="track-artist">Artista</h3>
        <h4 id="track-album">Álbum</h4>
        
        <audio id="audio" controls></audio>
        
        <div class="controls">
            <input type="file" id="file-input" accept="audio/mp3" multiple>
            <button id="prev-btn">⏮️</button>
            <button id="play-btn">▶️</button>
            <button id="next-btn">⏭️</button>
        </div>
        
        <div class="status-bar">
            <span id="current-time">00:00</span> / <span id="duration">00:00</span>
        </div>
    </div>
    
    <script src="script.js"></script>
</body>
</html>
body {
    font-family: Arial, sans-serif;
    background-color: #f0f0f0;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    margin: 0;
}

.player {
    background: white;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
    text-align: center;
}

.controls button {
    margin: 10px;
    padding: 10px;
    font-size: 24px;
    background: none;
    border: none;
    cursor: pointer;
    transition: color 0.3s;
}

.controls button:hover {
    color: #007bff;
}

.playlist {
    margin-top: 20px;
}

.playlist ul {
    list-style: none;
    padding: 0;
}

.playlist li {
    cursor: pointer;
    padding: 5px;
    transition: background 0.3s;
}

.playlist li:hover {
    background: #e0e0e0;
}
const audio = document.getElementById('audio');
const playBtn = document.getElementById('play-btn');
const prevBtn = document.getElementById('prev-btn');
const nextBtn = document.getElementById('next-btn');
const fileInput = document.getElementById('file-input');
const trackTitle = document.getElementById('track-title');
const trackArtist = document.getElementById('track-artist');
const trackAlbum = document.getElementById('track-album');
const currentTimeDisplay = document.getElementById('current-time');
const durationDisplay = document.getElementById('duration');

let tracks = [];
let currentTrackIndex = 0;

fileInput.addEventListener('change', (event) => {
    const files = event.target.files;
    tracks = Array.from(files).filter(file => file.type === 'audio/mp3');
    if (tracks.length > 0) {
        loadTrack(currentTrackIndex);
    }
});

playBtn.addEventListener('click', () => {
    if (audio.paused) {
        audio.play();
        playBtn.textContent = '⏸️'; // Cambiar a pausa
    } else {
        audio.pause();
        playBtn.textContent = '▶️'; // Cambiar a reproducir
    }
});

prevBtn.addEventListener('click', () => {
    currentTrackIndex = (currentTrackIndex - 1 + tracks.length) % tracks.length;
    loadTrack(currentTrackIndex);
});

nextBtn.addEventListener('click', () => {
    currentTrackIndex = (currentTrackIndex + 1) % tracks.length;
    loadTrack(currentTrackIndex);
});

audio.addEventListener('timeupdate', () => {
    const currentTime = Math.floor(audio.currentTime);
    const duration = Math.floor(audio.duration);
    currentTimeDisplay.textContent = formatTime(currentTime);
    durationDisplay.textContent = formatTime(duration);
});

function loadTrack(index) {
    const track = tracks[index];
    audio.src = URL.createObjectURL(track);
    trackTitle.textContent = track.name;
    trackArtist.textContent = "Artista Desconocido"; // Cambia esto según tu lógica
    trackAlbum.textContent = "Álbum Desconocido"; // Cambia esto según tu lógica
    audio.play();
    playBtn.textContent = '⏸️'; // Cambiar a pausa
}

function formatTime(seconds) {
    const minutes = Math.floor(seconds / 60);
    const secs = seconds % 60;
    return `${String(minutes).padStart(2, '0')}:${String(secs).padStart(2, '0')}`;
}

