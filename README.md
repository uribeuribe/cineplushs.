<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>CinePlus 🎬</title>

  <!-- Google AdSense -->
  <script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-3623005145407316"
     crossorigin="anonymous"></script>

  <style>
    body { margin:0; font-family:Arial,sans-serif; transition:background .3s, color .3s; }
    header { display:flex; justify-content:space-between; align-items:center;
             padding:15px; font-size:24px; font-weight:bold; }
    header span { cursor:pointer; padding:8px 12px; border-radius:4px; font-size:14px; }

    section { margin:20px; }
    h2 { border-left:5px solid #ff8c00; padding-left:10px; }
    .grid { display:grid; grid-template-columns:repeat(auto-fill,minmax(150px,1fr)); gap:15px; }
    .card { border-radius:5px; overflow:hidden; cursor:pointer; transition:.2s; }
    .card:hover { transform:scale(1.05); }
    .card img { width:100%; display:block; }

    .sticky-ad { position:fixed; bottom:0; left:0; width:100%; text-align:center; z-index:2000;
                 padding:5px 0; border-top:2px solid #ff8c00; }

    .search-bar { text-align:center; margin:15px; }
    .search-bar input { width:65%; padding:10px; font-size:16px; border:none; border-radius:5px; }
    .search-bar button { padding:10px 15px; background:#ff8c00; border:none; border-radius:5px;
                         font-weight:bold; cursor:pointer; }

    .genres { text-align:center; margin:10px; }
    .genres button { margin:5px; padding:8px 12px; border:none; border-radius:4px; cursor:pointer; }

    .modal { display:none; position:fixed; z-index:3000; left:0; top:0; width:100%; height:100%;
             background:rgba(0,0,0,0.9); justify-content:center; align-items:center; }
    .modal-content { width:80%; max-width:800px; }
    .close { position:absolute; top:20px; right:30px; font-size:30px; cursor:pointer; }

    /* 🎨 Modo oscuro */
    body.dark { background:#111; color:#fff; }
    body.dark header { background:#ff8c00; color:#fff; }
    body.dark .card { background:#222; }
    body.dark .genres button { background:#333; color:#fff; }
    body.dark .genres button:hover { background:#ff8c00; color:#000; }

    /* ☀️ Modo claro */
    body.light { background:#f5f5f5; color:#000; }
    body.light header { background:#ff8c00; color:#000; }
    body.light .card { background:#fff; border:1px solid #ddd; }
    body.light .genres button { background:#ddd; color:#000; }
    body.light .genres button:hover { background:#ff8c00; color:#fff; }
  </style>
</head>
<body class="dark">
  <header>
    🍿 CinePlus
    <span onclick="toggleTheme()">🌙/☀️</span>
  </header>

  <!-- 📌 Banner Superior -->
  <div style="text-align:center; margin:10px 0;">
    <ins class="adsbygoogle"
         style="display:block"
         data-ad-client="ca-pub-3623005145407316"
         data-ad-slot="4103131320"
         data-ad-format="auto"
         data-full-width-responsive="true"></ins>
    <script>(adsbygoogle = window.adsbygoogle || []).push({});</script>
  </div>

  <!-- Buscador -->
  <div class="search-bar">
    <input type="text" id="searchInput" placeholder="Buscar películas o series...">
    <button onclick="startSearch()">Buscar</button>
  </div>

  <!-- Filtros por género -->
  <div class="genres">
    <button onclick="loadGenre(28,'Acción')">Acción</button>
    <button onclick="loadGenre(35,'Comedia')">Comedia</button>
    <button onclick="loadGenre(27,'Terror')">Terror</button>
    <button onclick="loadGenre(18,'Drama')">Drama</button>
    <button onclick="loadGenre(878,'Ciencia Ficción')">Ciencia Ficción</button>
    <button onclick="loadGenre(16,'Animación')">Animación</button>
    <button onclick="loadGenre(99,'Documental')">Documental</button>
  </div>

  <!-- Resultados de búsqueda -->
  <section id="searchSection" style="display:none;">
    <h2>🔍 Resultados de búsqueda</h2>
    <div class="grid" id="searchResults"></div>
  </section>

  <!-- Resultados por género -->
  <section id="genreSection" style="display:none;">
    <h2 id="genreTitle">🎭 Género</h2>
    <div class="grid" id="genreResults"></div>
  </section>

  <!-- Categorías fijas -->
  <section>
    <h2>🔥 Películas Populares</h2>
    <div class="grid" id="popular"></div>
  </section>

  <!-- 📌 Anuncio intermedio -->
  <div style="text-align:center; margin:20px 0;">
    <ins class="adsbygoogle"
         style="display:block"
         data-ad-client="ca-pub-3623005145407316"
         data-ad-slot="4103131321"
         data-ad-format="auto"
         data-full-width-responsive="true"></ins>
    <script>(adsbygoogle = window.adsbygoogle || []).push({});</script>
  </div>

  <section>
    <h2>⭐ Mejor Valoradas</h2>
    <div class="grid" id="top_rated"></div>
  </section>

  <section>
    <h2>🎉 Estrenos</h2>
    <div class="grid" id="upcoming"></div>
  </section>

  <section>
    <h2>🎟️ En Cartelera</h2>
    <div class="grid" id="now_playing"></div>
  </section>

  <section>
    <h2>📺 Series Populares</h2>
    <div class="grid" id="tv_popular"></div>
  </section>

  <!-- 📌 Banner Sticky Inferior -->
  <div class="sticky-ad">
    <ins class="adsbygoogle"
         style="display:inline-block;width:100%;height:90px"
         data-ad-client="ca-pub-3623005145407316"
         data-ad-slot="4103131322"></ins>
    <script>(adsbygoogle = window.adsbygoogle || []).push({});</script>
  </div>

  <!-- Modal Trailer -->
  <div id="trailerModal" class="modal">
    <span class="close" onclick="closeTrailer()">&times;</span>
    <div class="modal-content">
      <iframe id="trailerFrame" width="100%" height="450" src="" frameborder="0" allowfullscreen></iframe>
    </div>
  </div>

<script>
// Tu código de JavaScript aquí (scroll infinito, carga de películas, trailers, etc.)
</script>

</body>
</html>
