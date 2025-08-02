<!DOCTYPE html>
<html lang="tr">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>MiniFlix</title>
<style>
  * {
    box-sizing: border-box;
    margin: 0; padding: 0;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  }
  body {
    background-color: #141414;
    color: #fff;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
  }
  header {
    background-color: #111;
    padding: 1rem 2rem;
    font-size: 1.8rem;
    font-weight: bold;
    letter-spacing: 2px;
    user-select: none;
  }
  nav {
    background-color: #181818;
    padding: 0.5rem 2rem;
    display: flex;
    gap: 1rem;
    overflow-x: auto;
  }
  nav button {
    background: none;
    border: none;
    color: #bbb;
    padding: 0.5rem 1rem;
    font-size: 1rem;
    cursor: pointer;
    border-radius: 4px;
    transition: background-color 0.3s, color 0.3s;
    white-space: nowrap;
  }
  nav button.active,
  nav button:hover {
    background-color: #e50914;
    color: white;
  }
  #search-container {
    padding: 0.5rem 2rem;
    background:#141414;
  }
  #search-input {
    width: 100%;
    max-width: 400px;
    padding: 0.5rem 1rem;
    font-size: 1rem;
    border-radius: 4px;
    border: none;
  }
  main {
    flex: 1;
    padding: 1rem 2rem;
  }
  .category-title {
    font-size: 1.5rem;
    margin-bottom: 0.75rem;
    letter-spacing: 1px;
  }
  .movie-row {
    display: flex;
    overflow-x: auto;
    gap: 1rem;
    padding-bottom: 1rem;
  }
  .movie-card {
    min-width: 140px;
    flex-shrink: 0;
    cursor: pointer;
    transition: transform 0.3s;
    border-radius: 6px;
    overflow: hidden;
    box-shadow: 0 2px 6px rgb(0 0 0 / 0.7);
    position: relative;
  }
  .movie-card:hover {
    transform: scale(1.05);
    box-shadow: 0 6px 12px rgb(229 9 20 / 0.9);
  }
  .movie-card img {
    width: 100%;
    display: block;
    height: 210px;
    object-fit: cover;
  }
  .movie-card p {
    padding: 0.4rem 0.2rem;
    font-size: 0.9rem;
    text-align: center;
    background-color: #222;
  }
  .movie-card .fav-btn {
    position: absolute;
    top: 6px;
    right: 6px;
    font-size: 1.4rem;
    color: #bbb;
    cursor: pointer;
    user-select: none;
    transition: color 0.3s;
  }
  .movie-card .fav-btn.active {
    color: #e50914;
  }

  /* Film detay popup */
  #detail-popup {
    position: fixed;
    top: 0; left: 0;
    width: 100vw; height: 100vh;
    background-color: rgba(0,0,0,0.85);
    display: none;
    justify-content: center;
    align-items: center;
    padding: 2rem;
    z-index: 100;
  }
  #detail-popup .content {
    background-color: #222;
    max-width: 800px;
    width: 90%;
    border-radius: 8px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
  }
  #detail-popup .content img {
    width: 100%;
    max-height: 400px;
    object-fit: cover;
  }
  #detail-popup .info {
    padding: 1rem 1.5rem;
    color: #ddd;
  }
  #detail-popup .info h2 {
    margin-bottom: 0.5rem;
    color: white;
  }
  #detail-popup .info p {
    margin-bottom: 1rem;
    font-size: 1rem;
    line-height: 1.3;
  }
  #detail-popup video {
    border-radius: 6px;
    background: black;
  }
  #detail-popup button.close-btn {
    background: #e50914;
    color: white;
    border: none;
    padding: 0.6rem 1rem;
    font-size: 1rem;
    cursor: pointer;
    border-radius: 4px;
    align-self: flex-end;
    margin: 0.5rem 1rem 1rem 0;
    transition: background-color 0.3s;
  }
  #detail-popup button.close-btn:hover {
    background: #b40710;
  }
  #detail-popup .video-controls {
    margin-top: 0.5rem;
    display: flex;
    gap: 1rem;
    align-items: center;
  }
  #detail-popup .video-controls button {
    font-size: 1.3rem;
    padding: 0.3rem 0.6rem;
    cursor: pointer;
    border-radius: 4px;
    border: none;
    background: #e50914;
    color: white;
    transition: background-color 0.3s;
  }
  #detail-popup .video-controls button:hover {
    background: #b40710;
  }

  /* Responsive */
  @media (max-width: 768px) {
    .movie-card {
      min-width: 110px;
    }
    .movie-card img {
      height: 160px;
    }
  }
  @media (max-width: 480px) {
    nav {
      padding: 0.5rem 1rem;
    }
    main {
      padding: 1rem;
    }
    .movie-card {
      min-width: 90px;
    }
    .movie-card img {
      height: 120px;
    }
  }

  /* Login screen */
  #login-screen {
    position: fixed;
    top: 0; left: 0;
    width: 100vw; height: 100vh;
    background: #141414;
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 200;
  }
  #login-form {
    background: #222;
    padding: 2rem;
    border-radius: 8px;
    color: #fff;
    display: flex;
    flex-direction: column;
    gap: 1rem;
    width: 300px;
  }
  #login-form h2 {
    margin-bottom: 0.5rem;
  }
  #login-form input {
    padding: 0.5rem;
    border: none;
    border-radius: 4px;
    font-size: 1rem;
  }
  #login-form button {
    background: #e50914;
    border: none;
    color: #fff;
    padding: 0.6rem;
    border-radius: 4px;
    font-size: 1.1rem;
    cursor: pointer;
  }
  #login-form button:hover {
    background: #b40710;
  }
</style>
</head>
<body>

<!-- Giriş ekranı -->
<div id="login-screen">
  <form id="login-form">
    <h2>Kullanıcı Girişi</h2>
    <input type="text" id="username" placeholder="İsminizi yazın" required />
    <button type="submit">Giriş Yap</button>
  </form>
</div>

<header>MiniFlix</header>

<nav>
  <button class="active" data-category="anime">Anime</button>
  <button data-category="scifi">Bilim Kurgu</button>
  <button data-category="adventure">Macera</button>
  <button data-category="series">Diziler</button>
  <button data-category="movies">Filmler</button>
  <button data-category="favorites">Favoriler</button>
</nav>

<div id="search-container">
  <input type="text" id="search-input" placeholder="Film veya kategori ara..." />
</div>

<main>
  <h2 class="category-title">Anime</h2>
  <div class="movie-row" id="movie-row"></div>
</main>

<!-- Film detay popup -->
<div id="detail-popup">
  <div class="content">
    <button class="close-btn">Kapat</button>
    <img src="" alt="Film Görseli" id="detail-img" />
    <div class="info">
      <h2 id="detail-title"></h2>
      <p id="detail-description"></p>
      <video id="detail-video" width="100%" controls style="margin-top: 1rem; display:none;">
        <source src="" type="video/mp4" />
        Tarayıcınız video etiketini desteklemiyor.
      </video>
      <div class="video-controls">
        <button id="fullscreen-btn" title="Tam ekran">⛶</button>
        <button id="mute-btn" title="Ses aç/kapat">🔊</button>
      </div>
    </div>
  </div>
</div>

<script>
  // Kullanıcı girişi
  const loginScreen = document.getElementById('login-screen');
  const loginForm = document.getElementById('login-form');
  const usernameInput = document.getElementById('username');

  function checkLogin() {
    const savedUser = localStorage.getItem('miniflixUser');
    if(savedUser){
      loginScreen.style.display = 'none';
    } else {
      loginScreen.style.display = 'flex';
    }
  }

  loginForm.addEventListener('submit', e => {
    e.preventDefault();
    const username = usernameInput.value.trim();
    if(username.length > 0){
      localStorage.setItem('miniflixUser', username);
      loginScreen.style.display = 'none';
    }
  });

  checkLogin();

  // Kategoriler ve içerik
  const categories = {
    anime: [
      {
        title: "Attack on Titan",
        img: "https://upload.wikimedia.org/wikipedia/en/7/79/Shingeki_no_Kyojin_manga_volume_1_cover.jpg",
        description: "Devasa devlere karşı insanlığın savaşı.",
        video: "https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/ElephantsDream.mp4"
      },
      {
        title: "Naruto",
        img: "https://upload.wikimedia.org/wikipedia/en/9/94/NarutoCoverTankobon1.jpg",
        description: "Bir ninja olma hayaliyle dolu genç bir savaşçının hikayesi.",
        video: "https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/ForBiggerJoyrides.mp4"
      },
      {
        title: "One Piece",
        img: "https://upload.wikimedia.org/wikipedia/en/6/6b/One_Piece_Logo.svg",
        description: "Denizlerde macera dolu korsanların hikayesi.",
        video: "https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/Sintel.mp4"
      }
    ],
    scifi: [
      {
        title: "Black Mirror",
        img: "https://upload.wikimedia.org/wikipedia/en/0/0a/Black_Mirror_Title_Card.png",
        description: "Teknolojinin karanlık yüzünü anlatan diziler."
      },
      {
        title: "Stranger Things",
        img: "https://upload.wikimedia.org/wikipedia/en/f/f7/Stranger_Things_logo.png",
        description: "Gizemli olaylar ve paralel evrenler."
      },
      {
        title: "Westworld",
        img: "https://upload.wikimedia.org/wikipedia/en/e/e0/Westworld_logo.png",
        description: "Yapay zekanın sınırlarında geçen hikayeler."
      }
    ],
    adventure: [
      {
        title: "The Mandalorian",
        img: "https://upload.wikimedia.org/wikipedia/en/c/cb/The_Mandalorian_logo.png",
        description: "Uzayda geçen bir ödül avcısının maceraları."
      },
      {
        title: "Pirates of the Caribbean",
        img: "https://upload.wikimedia.org/wikipedia/en/8/87/Pirates_of_the_Caribbean_-_The_Curse_of_the_Black_Pearl.png",
        description: "Denizlerde geçen efsanevi korsan hikayesi."
      },
      {
        title: "Indiana Jones",
        img: "https://upload.wikimedia.org/wikipedia/en/e/e3/Indiana_Jones_logo.svg",
        description: "Macera dolu arkeoloji ve keşif yolculuğu."
      }
    ],
    series: [
      {
        title: "Breaking Bad",
        img: "https://upload.wikimedia.org/wikipedia/en/6/61/Breaking_Bad_title_card.png",
        description: "Kimya öğretmeninin suç dünyasına girişi."
      },
      {
        title: "Game of Thrones",
        img: "https://upload.wikimedia.org/wikipedia/en/d/d8/Game_of_Thrones_title_card.jpg",
        description: "Krallıkların taht savaşı."
      },
      {
        title: "Friends",
        img: "https://upload.wikimedia.org/wikipedia/en/b/bc/Friends_logo.svg",
        description: "Bir grup arkadaşın günlük hayatları."
      }
    ],
    movies: [
      {
        title: "Inception",
        img: "https://upload.wikimedia.org/wikipedia/en/7/7f/Inception_ver3.jpg",
        description: "Rüya içinde rüya dünyası."
      },
      {
        title: "The Dark Knight",
        img: "https://upload.wikimedia.org/wikipedia/en/8/8a/Dark_Knight.jpg",
        description: "Batman ve Gotham'ın hikayesi."
      },
      {
        title: "Interstellar",
        img: "https://upload.wikimedia.org/wikipedia/en/b/bc/Interstellar_film_poster.jpg",
        description: "Uzayın derinliklerinde insanlık mücadelesi."
      }
    ]
  };

  // Favoriler yönetimi
  let favorites = JSON.parse(localStorage.getItem('miniflixFavorites')) || [];

  function saveFavorites() {
    localStorage.setItem('miniflixFavorites', JSON.stringify(favorites));
  }

  function isFavorite(title) {
    return favorites.includes(title);
  }

  function toggleFavorite(title, btn) {
    if(isFavorite(title)){
      favorites = favorites.filter(fav => fav !== title);
      btn.classList.remove('active');
    } else {
      favorites.push(title);
      btn.classList.add('active');
    }
    saveFavorites();
  }

  // Elemanlar
  const navButtons = document.querySelectorAll('nav button');
  const movieRow = document.getElementById('movie-row');
  const categoryTitle = document.querySelector('.category-title');
  const searchInput = document.getElementById('search-input');

  function createMovieCard(movie){
    const card = document.createElement('div');
    card.className = 'movie-card';
    card.innerHTML = `
      <img src="${movie.img}" alt="${movie.title}" />
      <p>${movie.title}</p>
      <div class="fav-btn ${isFavorite(movie.title) ? 'active' : ''}">♥</div>
    `;
    card.querySelector('.fav-btn').addEventListener('click', e => {
      e.stopPropagation();
      toggleFavorite(movie.title, e.target);
    });
    card.addEventListener('click', () => showDetail(movie));
    return card;
  }

  function loadCategory(category) {
    categoryTitle.textContent = navButtonsArray.find(b => b.dataset.category === category).textContent;
    movieRow.innerHTML = '';

    if(category === 'favorites'){
      const favMovies = [];
      Object.values(categories).forEach(catArr => {
        catArr.forEach(movie => {
          if(isFavorite(movie.title)) favMovies.push(movie);
        });
      });
      if(favMovies.length === 0){
        movieRow.innerHTML = '<p style="color:#bbb;">Favori film yok.</p>';
        return;
      }
      favMovies.forEach(movie => {
        const card = createMovieCard(movie);
        movieRow.appendChild(card);
      });
      return;
    }

    categories[category].forEach(movie => {
      const card = createMovieCard(movie);
      movieRow.appendChild(card);
    });
  }

  function showDetail(movie) {
    document.getElementById('detail-img').src = movie.img;
    document.getElementById('detail-title').textContent = movie.title;
    document.getElementById('detail-description').textContent = movie.description;

    const video = document.getElementById('detail-video');
    if(movie.video){
      video.style.display = "block";
      video.src = movie.video;
      video.load();
      muteBtn.textContent = video.muted ? '🔇' : '🔊';
    } else {
      video.style.display = "none";
      video.pause();
      video.src = "";
    }

    document.getElementById('detail-popup').style.display = 'flex';
  }

  document.querySelector('#detail-popup .close-btn').addEventListener('click', () => {
    const video = document.getElementById('detail-video');
    video.pause();
    video.src = "";
    document.getElementById('detail-popup').style.display = 'none';
  });

  // Aktif buton yönetimi
  const navButtonsArray = Array.from(navButtons);
  navButtons.forEach(button => {
    button.addEventListener('click', () => {
      navButtons.forEach(b => b.classList.remove('active'));
      button.classList.add('active');
      loadCategory(button.dataset.category);
      searchInput.value = '';
    });
  });

  // Arama
  searchInput.addEventListener('input', () => {
    const query = searchInput.value.toLowerCase().trim();

    if(query === ''){
