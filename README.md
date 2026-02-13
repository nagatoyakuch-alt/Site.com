# Site.com<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Meu Site de Fotos e Vídeos</title>
<style>
  body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
    background-color: #f2f2f2;
  }
  header {
    background-color: #4CAF50;
    color: white;
    padding: 15px;
    text-align: center;
  }
  .container {
    padding: 20px;
    max-width: 500px;
    margin: auto;
    background-color: white;
    border-radius: 10px;
  }
  input[type="text"] {
    padding: 8px;
    width: 70%;
    margin-right: 5px;
    border-radius: 5px;
    border: 1px solid #ccc;
  }
  button {
    padding: 8px 12px;
    margin: 5px;
    cursor: pointer;
    border-radius: 5px;
    border: none;
    background-color: #4CAF50;
    color: white;
  }
  video, img {
    max-width: 100%;
    margin-top: 10px;
    border-radius: 10px;
  }
  .post {
    border-bottom: 1px solid #ccc;
    padding: 10px 0;
  }
</style>
</head>
<body>

<header>
  <h1>Meu Site de Fotos e Vídeos</h1>
</header>

<div class="container">

  <!-- Barra de pesquisa simulada -->
  <div>
    <input type="text" id="searchInput" placeholder="Pesquisar...">
    <button onclick="search()">Pesquisar</button>
    <p id="searchResult"></p>
  </div>

  <!-- Botões de permissão -->
  <div>
    <button onclick="requestCamera()">Permissão Câmera</button>
    <button onclick="requestMicrophone()">Permissão Microfone</button>
  </div>

  <!-- Câmera e microfone -->
  <video id="cameraVideo" autoplay style="display:none;"></video>

  <!-- Publicar fotos -->
  <div>
    <input type="file" id="fileInput" accept="image/*,video/*">
    <button onclick="postFile()">Publicar</button>
  </div>

  <!-- Área de posts -->
  <div id="posts"></div>
</div>

<script>
  // Pesquisa simulada
  function search() {
    const query = document.getElementById('searchInput').value;
    const result = document.getElementById('searchResult');
    if(query.trim() === '') {
      result.textContent = "Digite algo para pesquisar!";
    } else {
      result.textContent = `Você pesquisou: "${query}" (simulação)`;
    }
  }

  // Permissão câmera
  function requestCamera() {
    navigator.mediaDevices.getUserMedia({ video: true })
    .then(stream => {
      const video = document.getElementById('cameraVideo');
      video.style.display = "block";
      video.srcObject = stream;
    })
    .catch(err => alert("Permissão de câmera negada ou erro: " + err));
  }

  // Permissão microfone
  function requestMicrophone() {
    navigator.mediaDevices.getUserMedia({ audio: true })
    .then(stream => alert("Microfone permitido!"))
    .catch(err => alert("Permissão de microfone negada ou erro: " + err));
  }

  // Publicar fotos ou vídeos
  function postFile() {
    const input = document.getElementById('fileInput');
    const posts = document.getElementById('posts');

    if(input.files.length === 0) {
      alert("Selecione um arquivo para publicar!");
      return;
    }

    const file = input.files[0];
    const reader = new FileReader();
    reader.onload = function(e) {
      const div = document.createElement('div');
      div.className = 'post';

      if(file.type.startsWith('image/')) {
        const img = document.createElement('img');
        img.src = e.target.result;
        div.appendChild(img);
      } else if(file.type.startsWith('video/')) {
        const video = document.createElement('video');
        video.src = e.target.result;
        video.controls = true;
        div.appendChild(video);
      }

      posts.prepend(div); // adiciona no topo
      input.value = ''; // limpa o input
    }

    reader.readAsDataURL(file);
  }
</script>

</body>
</html>
