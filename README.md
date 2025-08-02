<!DOCTYPE html>
<html lang="tr">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>MiniFlix</title>
  <style>
    body {
      background:#141414;
      color:#eee;
      font-family: Arial, sans-serif;
      margin:0; padding:0;
      display: flex;
      flex-direction: column;
      min-height: 100vh;
    }
    #login-screen {
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      background:#000;
    }
    #login-screen form {
      background:#222;
      padding: 2rem;
      border-radius: 8px;
      box-shadow: 0 0 10px #e50914;
      width: 300px;
      display: flex;
      flex-direction: column;
      gap: 1rem;
    }
    #login-screen input {
      padding: 0.5rem;
      border-radius: 4px;
      border:none;
      outline:none;
      font-size: 1rem;
    }
    #login-screen button {
      background:#e50914;
      border:none;
      color:#fff;
      padding:0.7rem;
      cursor:pointer;
      font-weight: bold;
      border-radius: 4px;
    }
    header {
      font-size: 2rem;
      padding: 1rem;
      background:#111;
      text-align: center;
      font-weight: bold;
      user-select:none;
    }
    nav {
      display: flex;
      gap: 0.5rem;
      background:#222;
      padding: 0.5rem;
      justify-content: center;
      flex-wrap: wrap;
    }
    nav button {
      background:#333;
      border:none;
      color:#ddd;
      padding: 0.5rem 1rem;
      cursor:pointer;
      border-radius: 4px;
      font-weight: 600;
      position: relative;
    }
    nav button.active {
      background:#e50914;
      color:#fff;
    }
    #search-container {
      display: flex;
      justify-content: center;
      padding: 1rem;
      background:#141414;
    }
    #search-input {
      width: 80%;
      padding: 0.5rem 1rem;
      font-size: 1rem;
      border-radius: 4px;
      border: none;
      outline:none;
    }
    main {
      flex-grow: 1;
      padding: 1rem;
      background:#141414;
    }
    .category-title {
      margin-bottom: 1rem;
      font-size: 1.5rem;
      text-align: center;
    }
    .movie-row {
      display: flex;
      gap: 1rem;
      flex-wrap: wrap;
      justify-content: center;
    }
    .movie-card {
      background:#222;
      border-radius: 6px;
      width: 150px;
      cursor: pointer;
      display: flex;
      flex-direction: column;
      align-items: center;
      transition: transform 0.2s;
      position: relative;
    }
    .movie-card:hover {
      transform: scale(1.05);
    }
    .movie-card img {
      width: 100%;
      border-radius: 6px 6px 0 0;
      height: 220px;
      object-fit: cover;
    }
    .movie-card h3 {
      font-size: 1rem;
      margin: 0.5rem 0 0.3rem 0;
      text-align: center;
    }
    .fav-btn {
      position: absolute;
      top: 8px;
      right: 8px;
      background: rgba(0,0,0,0.6);
      border: none;
      color: #ccc;
      font-size: 1.2rem;
      cursor: pointer;
      border-radius: 50%;
      width: 28px;
      height: 28px;
      display: flex;
      justify-content: center;
      align-items: center;
      transition: color 0.3s;
      user-select:none;
    }
    .fav-btn.active {
      color: #e50914;
    }
    #detail-popup {
      display: none;
      position: fixed;
      inset: 0;
      background: rgba(0,0,0,0.85);
      justify-content: center;
      align-items: center;
      z-index: 1000;
    }
    #detail-popup .detail-content {
      background: #222;
      padding: 1rem;
      border-radius: 8px;
      max-width: 400px;
      max-height: 80vh;
      overflow-y: auto;
      position: relative;
      color: #eee;
      text-align: center;
    }
    #detail-popup img {
      max-width: 100%;
      border-radius: 6px;
      margin-bottom: 1rem;
    }
    #detail-popup h2 {
      margin-bottom: 0.5rem;
    }
    #detail-popup p {
      font-size: 1rem;
      margin-bottom: 1rem;
    }
    #close-detail {
      position: absolute;
      top: 8px;
      right: 12px;
      background: none;
      border: none;
      font-size: 1.5rem;
      color: #eee;
      cursor: pointer;
      user-select:none;
    }
    #detail-fav-btn {
      background: #e50914;
      border: none;
      padding: 0.5rem 1rem;
      color: white;
      font-weight: bold;
      border-radius: 4px;
      cursor: pointer;
    }
    /* Admin Panel */
    #admin-panel {
      display: none;
      background: #333;
      color: #eee;
      padding: 1rem;
      margin: 1rem;
      border-radius: 6px;
    }
    #admin-panel button {
      margin-right: 0.7rem;
      background: #e50914;
      border: none;
      color: white;
      padding: 0.5rem 1rem;
      border-radius: 4px;
      cursor: pointer;
      font-weight: 600;
    }
    #admin-users-list > div {
      margin-bottom: 0.5rem;
      background: #444;
      padding: 0.5rem;
      border-radius: 4px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    #admin-users-list button {
      background: #222;
      border: none;
      padding: 0.3rem 0.7rem;
      border-radius: 4px;
      cursor: pointer;
      color: #eee;
    }
    /* Film Silme Butonu */
    .delete-btn {
      position: absolute;
      top: 8px;
      left: 8px;
      background-color: #e50914;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
      padding: 2px 6px;
      font-size: 0.8rem;
      user-select:none;
    }
  </style>
</head>
<body>
  <!-- Login ekranı -->
  <div id="login-screen" style="display: flex;">
    <form id="login-form">
      <h2>Kullanıcı Girişi</h2>
      <input type="email" id="email" placeholder="Email" required />
      <input type="password" id="password" placeholder="Şifre" required />
      <button type="submit">Giriş Yap</button>
      <p style="color:#bbb;font-size:0.9rem;">
        Hesabınız yok mu? <a href="#" id="register-link" style="color:#e50914;">Kayıt ol</a>
      </p>
    </form>
  </div>

  <!-- Film arayüzü ve admin panel -->
  <div id="app-content" style="display:none;">
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

    <!-- Admin Panel -->
    <div id="admin-panel">
      <h3>Yönetici Paneli</h3>
      <button id="btn-refresh-data">Verileri Yenile</button>
      <button id="btn-clear-favorites">Tüm Favorileri Temizle</button>
      <h4>Kullanıcılar</h4>
      <div id="admin-users-list"></div>

      <h4>Film Ekle</h4>
      <form id="add-movie-form">
        <input type="text" id="movie-title" placeholder="Film Adı" required />
        <input type="number" id="movie-year" placeholder="Yıl" required />
        <input type="text" id="movie-category" placeholder="Kategori (anime, scifi, adventure, series, movies)" required />
        <input type="text" id="movie-image" placeholder="Poster URL" required />
        <textarea id="movie-description" placeholder="Açıklama" required></textarea>
        <button type="submit">Ekle</button>
      </form>
      <hr />
    </div>

    <div id="detail-popup"></div>
  </div>

  <script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/12.0.0/firebase-app.js";
    import {
      getAuth,
      signInWithEmailAndPassword,
      createUserWithEmailAndPassword,
      onAuthStateChanged,
      signOut
    } from "https://www.gstatic.com/firebasejs/12.0.0/firebase-auth.js";
    import {
      getFirestore,
      doc,
      setDoc,
      getDoc,
      collection,
      getDocs,
      addDoc,
      deleteDoc,
      updateDoc
    } from "https://www.gstatic.com/firebasejs/12.0.0/firebase-firestore.js";

    // Firebase config
    const firebaseConfig = {
      apiKey: "AIzaSyCqpQpBqiys2gSNJowq74BLf_XgxI18N2I",
      authDomain: "film-91c18.firebaseapp.com",
      projectId: "film-91c18",
      storageBucket: "film-91c18.appspot.com",
      messagingSenderId: "670730269608",
      appId: "1:670730269608:web:b59ffbbc554a8a90f9e39d"
    };

    const app = initializeApp(firebaseConfig);
    const auth = getAuth(app);
    const db = getFirestore(app);

    // Admin UID
    const adminUID = "u7jahcWw14NPXueX6MqSLuojBXW2"; // Kendi UID'nle değiştir!

    // Favori listesi
    let favorites = [];

    // DOM elemanları
    const loginScreen = document.getElementById('login-screen');
    const appContent = document.getElementById('app-content');
    const loginForm = document.getElementById('login-form');
    const emailInput = document.getElementById('email');
    const passwordInput = document.getElementById('password');
    const registerLink = document.getElementById('register-link');
    const movieRow = document.getElementById('movie-row');
    const categoryTitle = document.querySelector('.category-title');
    const searchInput = document.getElementById('search-input');
    const detailPopup = document.getElementById('detail-popup');
    const adminPanel = document.getElementById('admin-panel');
    const adminUsersList = document.getElementById('admin-users-list');
    const addMovieForm = document.getElementById('add-movie-form');

    let isRegistering = false;
    let currentCategory = 'anime';

    // Kayıt linki tıklaması
    registerLink.addEventListener('click', e => {
      e.preventDefault();
      isRegistering = true;
      loginForm.querySelector('h2').textContent = "Kayıt Ol";
      loginForm.querySelector('button').textContent = "Kayıt Ol";
      registerLink.style.display = 'none';
    });

    // Giriş/Kayıt form submit
    loginForm.addEventListener('submit', async (e) => {
      e.preventDefault();
      const email = emailInput.value.trim();
      const password = passwordInput.value.trim();

      try {
        if (isRegistering) {
          const cred = await createUserWithEmailAndPassword(auth, email, password);
          await setDoc(doc(db, "users", cred.user.uid), {
            email: email,
            createdAt: new Date(),
            isBanned: false,
            lastSeen: new Date()
          });
          alert("Kayıt başarılı! Şimdi giriş yapabilirsiniz.");
          isRegistering = false;
          loginForm.querySelector('h2').textContent = "Kullanıcı Girişi";
          loginForm.querySelector('button').textContent = "Giriş Yap";
          registerLink.style.display = 'inline';
          loginForm.reset();
        } else {
          await signInWithEmailAndPassword(auth, email, password);
        }
      } catch (err) {
        alert("Hata: " + err.message);
      }
    });

    // Kullanıcı durumunu dinle
    onAuthStateChanged(auth, async (user) => {
      if (user) {
        // Kullanıcı Firestore kaydı varsa, lastSeen güncelle
        const userDoc = doc(db, "users", user.uid);
        await setDoc(userDoc, { lastSeen: new Date() }, { merge: true });

        if (user.uid === adminUID) {
          adminPanel.style.display = 'block';
          loadUsers();
        } else {
          adminPanel.style.display = 'none';
        }

        loginScreen.style.display = 'none';
        appContent.style.display = 'block';

        // Engelleme kontrolü
        const snap = await getDoc(userDoc);
        if (snap.exists() && snap.data().isBanned) {
          alert("Hesabınız engellenmiştir. Lütfen yönetici ile iletişime geçin.");
          await signOut(auth);
          return;
        }

        await loadFavorites();
        await loadMovies(currentCategory);
      } else {
        loginScreen.style.display = 'flex';
        appContent.style.display = 'none';
        adminPanel.style.display = 'none';
        favorites = [];
        movieRow.innerHTML = '';
      }
    });

    // Favorileri Firestore'a kaydet
    async function saveFavorites() {
      const user = auth.currentUser;
      if (!user) return;
      await setDoc(doc(db, "favorites", user.uid), { list: favorites });
    }

    // Favorileri Firestore'dan yükle
    async function loadFavorites() {
      const user = auth.currentUser;
      if (!user) return;
      const snap = await getDoc(doc(db, "favorites", user.uid));
      if (snap.exists()) {
        favorites = snap.data().list;
      } else {
        favorites = [];
      }
    }

    // Favori kontrolü
    function isFavorite(title) {
      return favorites.includes(title);
    }

    // Favori ekle/çıkar
    function toggleFavorite(title, btn) {
      if (isFavorite(title)) {
        favorites = favorites.filter(f => f !== title);
        btn.classList.remove('active');
      } else {
        favorites.push(title);
        btn.classList.add('active');
      }
      saveFavorites();
    }

    // Film kartı oluştur
    function createMovieCard(movie) {
      const card = document.createElement('div');
      card.className = 'movie-card';
      card.innerHTML = `
        <img src="${movie.image}" alt="${movie.title}" />
        <h3>${movie.title} (${movie.year})</h3>
        <button class="fav-btn ${isFavorite(movie.title) ? 'active' : ''}">❤</button>
      `;

      const favBtn = card.querySelector('.fav-btn');
      favBtn.addEventListener('click', e => {
        e.stopPropagation();
        toggleFavorite(movie.title, favBtn);
      });

      card.addEventListener('click', () => {
        showDetail(movie);
      });

      if (auth.currentUser && auth.currentUser.uid === adminUID) {
        const delBtn = document.createElement('button');
        delBtn.textContent = 'Sil';
        delBtn.className = 'delete-btn';
        delBtn.addEventListener('click', async (e) => {
          e.stopPropagation();
          if (confirm(`"${movie.title}" filmini silmek istediğine emin misin?`)) {
            await deleteDoc(doc(db, "movies", movie.id));
            alert("Film silindi.");
            loadMovies(currentCategory);
          }
        });
        card.appendChild(delBtn);
      }

      return card;
    }

    // Filmleri Firestore’dan yükle
    async function loadMovies(category) {
      currentCategory = category;
      categoryTitle.textContent = category.charAt(0).toUpperCase() + category.slice(1);
      movieRow.innerHTML = '';

      const querySnapshot = await getDocs(collection(db, "movies"));
      const movies = [];
      querySnapshot.forEach(docSnap => {
        const data = docSnap.data();
        data.id = docSnap.id;
        if (category === 'favorites') {
          if (favorites.includes(data.title)) movies.push(data);
        } else if (data.category === category) {
          movies.push(data);
        }
      });

      movies.forEach(movie => {
        const card = createMovieCard(movie);
        movieRow.appendChild(card);
      });
    }

    // Detay popup göster
    function showDetail(movie)
