<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>My Music Playlist</title>

<style>
* {
    box-sizing: border-box;
}

body {
    margin: 0;
    font-family: Arial;
    background: #101018;
    color: white;
    padding: 20px;
}

.container {
    max-width: 500px;
    margin: auto;
}

h1 {
    text-align: center;
    color: #a78bfa;
}

.player {
    background: #20202c;
    padding: 20px;
    border-radius: 20px;
    text-align: center;
}

.cover {
    width: 220px;
    height: 220px;
    object-fit: cover;
    border-radius: 15px;
}

button {
    background: #8b5cf6;
    border: none;
    color: white;
    padding: 12px;
    border-radius: 10px;
    margin: 5px;
    cursor: pointer;
}

input {
    width: 100%;
    padding: 12px;
    border-radius: 10px;
    border: none;
    margin: 15px 0;
}

.song {
    background: #292938;
    padding: 15px;
    margin: 8px 0;
    border-radius: 10px;
    cursor: pointer;
}

.song:hover {
    background: #514079;
}
</style>
</head>

<body>

<div class="container">

<h1>🎧 MY MUSIC</h1>

<div class="player">

<img id="cover" class="cover"
src="covers/cover1.jpg">

<h2 id="title">My Playlist</h2>

<audio id="audio"></audio>

<button onclick="previous()">⏮</button>
<button onclick="togglePlay()" id="play">▶</button>
<button onclick="next()">⏭</button>
<button onclick="shuffle()">🔀</button>

<br><br>

<input type="range" id="progress"
value="0" min="0" max="100">

<div id="time">00:00 / 00:00</div>

</div>

<h2>My Songs</h2>

<input type="text"
id="search"
placeholder="Search music..."
oninput="showSongs()">

<div id="playlist"></div>

</div>

<script>

const songs = [
{
title: "Song 1",
file: "music/song1.mp3",
cover: "covers/cover1.jpg"
},
{
title: "Song 2",
file: "music/song2.mp3",
cover: "covers/cover2.jpg"
},
{
title: "Song 3",
file: "music/song3.mp3",
cover: "covers/cover1.jpg"
}
];

let current = 0;

const audio = document.getElementById("audio");

function loadSong(index) {
    current = index;

    audio.src = songs[current].file;

    document.getElementById("title").innerText =
    songs[current].title;

    document.getElementById("cover").src =
    songs[current].cover;

    audio.play();
    document.getElementById("play").innerText = "⏸";
}

function togglePlay() {
    if(audio.paused) {
        audio.play();
        document.getElementById("play").innerText = "⏸";
    } else {
        audio.pause();
        document.getElementById("play").innerText = "▶";
    }
}

function next() {
    loadSong((current + 1) % songs.length);
}

function previous() {
    loadSong((current - 1 + songs.length) % songs.length);
}

function shuffle() {
    let random = Math.floor(Math.random() * songs.length);
    loadSong(random);
}

function showSongs() {
    let search = document.getElementById("search").value.toLowerCase();

    document.getElementById("playlist").innerHTML = "";

    songs.forEach((song, index) => {
        if(song.title.toLowerCase().includes(search)) {
            let div = document.createElement("div");
            div.className = "song";
            div.textContent = "🎵 " + song.title;
            div.onclick = () => loadSong(index);

            document.getElementById("playlist").appendChild(div);
        }
    });
}

audio.ontimeupdate = () => {
    if(audio.duration) {
        document.getElementById("progress").value =
        (audio.currentTime / audio.duration) * 100;

        let format = t => {
            let m = Math.floor(t / 60);
            let s = Math.floor(t % 60);
            return m + ":" + String(s).padStart(2, "0");
        };

        document.getElementById("time").innerText =
        format(audio.currentTime) + " / " +
        format(audio.duration);
    }
};

document.getElementById("progress").oninput = e => {
    if(audio.duration) {
        audio.currentTime =
        (e.target.value / 100) * audio.duration;
    }
};

audio.onended = next;

showSongs();

</script>

</body>
</html>