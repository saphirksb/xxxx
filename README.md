<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>XIXOFLIX</title>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css" rel="stylesheet">
    <style>
        body {
            margin: 0;
            height: auto;
            background-color: #3498db;
            font-family: Arial, sans-serif;
            overflow-y: scroll;
        }

        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            padding: 10px 20px;
            background-color: rgba(0, 0, 0, 0.5);
            z-index: 10;
        }

        .site-name {
            font-size: 40px;
            font-weight: bold;
            color: white;
            letter-spacing: 2px;
        }

        .menu {
            font-size: 18px;
            color: white;
            display: flex;
            gap: 30px;
        }

        .menu a {
            text-decoration: none;
            color: white;
        }

        .search-bar {
            display: none;
            padding: 5px 15px;
            font-size: 16px;
            border: none;
            border-radius: 5px;
            width: 200px;
            margin-right: 20px;
        }

        .search-icon {
            font-size: 20px;
            color: white;
            cursor: pointer;
        }

        .login-btn {
            font-size: 18px;
            color: white;
            display: flex;
            align-items: center;
            gap: 10px;
            cursor: pointer;
            border: 1px solid white;
            padding: 10px 20px;
            border-radius: 5px;
            transition: background-color 0.3s, color 0.3s;
        }

        .login-btn:hover {
            background-color: #f39c12;
            color: #3498db;
        }

        .hero {
            position: relative;
            height: 500px;
            width: 100%;
            margin: 0 20px;
            overflow: hidden;
            margin-top: 70px;
            transition: opacity 0.3s ease;
        }

        .hero img {
            width: calc(100% - 20px);
            height: 100%;
            object-fit: cover;
            object-position: center;
            border-radius: 15px;
        }

        .film-trending {
            text-align: left;
            color: white;
            font-size: 24px;
            margin: 20px 20px 10px;
            font-weight: bold;
        }

        .image-gallery {
            display: flex;
            justify-content: space-between;
            margin: 0 20px;
            gap: 10px;
            flex-wrap: wrap;
            overflow-x: auto;
            transition: transform 0.5s ease-in-out;
        }

        .image-gallery img {
            width: 13%;
            height: auto;
            border-radius: 10px;
            object-fit: cover;
            transition: transform 0.5s ease-in-out;
        }

        .watch-now-btn {
            position: absolute;
            bottom: 30px;
            left: 30px;
            padding: 10px 20px;
            background-color: #e50914;
            color: white;
            border: none;
            border-radius: 5px;
            font-size: 18px;
            cursor: pointer;
        }

        .watch-now-btn:hover {
            background-color: #f40612;
        }

        /* Masquer le film au départ */
        .hidden {
            display: none;
        }

        /* Positionner le film centré */
        .movie-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.8);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 20;
            display: none;
        }

        .movie-container video {
            width: 80%;
            height: auto;
            border-radius: 15px;
        }

        .movie-container .close-btn {
            position: absolute;
            top: 20px;
            right: 20px;
            font-size: 30px;
            color: white;
            cursor: pointer;
        }

        /* Ajout du style pour masquer les suggestions */
        .hidden-suggestions {
            display: none;
        }
    </style>
</head>
<body>

    <!-- Conteneur du titre et du menu -->
    <div class="background">
        <div class="header">
            <div class="site-name">XIXOFLIX</div>
            <div class="menu">
                <a href="#">HOME</a>
                <a href="#">FILM</a>
                <a href="#">EN DIRECT</a>
            </div>
            <div style="display: flex; align-items: center;">
                <input type="text" class="search-bar" id="searchBar" placeholder="Rechercher..." oninput="handleSearchInput()">
                <i class="fas fa-search search-icon" onclick="toggleSearch()"></i>
                <div class="login-btn">
                    <i class="fas fa-user"></i>
                    <span>S'identifier</span>
                </div>
            </div>
        </div>

        <!-- Section de mise en avant avec l'image -->
        <div class="hero" id="heroSection">
            <img src="movie-joker-dc-comics-logo-hd-wallpaper-preview.jpg" alt="Film en avant-première">
            <div class="hero-content">
                <button class="watch-now-btn" onclick="openMovie()">Regarder maintenant</button>
            </div>
        </div>

        <!-- Film en tendance -->
        <div class="film-trending">
            FILM EN TENDANCE
        </div>

        <!-- Galerie d'images -->
        <div class="image-gallery" id="imageGallery">
            <img src="images (2).jpg" alt="Joker" class="joker-suggestion suggestion">
            <img src="téléchargement (2).jpg" alt="Demon Slayer" class="demon-slayer-suggestion suggestion">
            <img src="moi_moche_et_mechant.jpg" alt="Image 3" class="suggestion">
            <img src="images (3).jpg" alt="Image 4" class="suggestion">
            <img src="images (4).jpg" alt="Image 5" class="suggestion">
            <img src="images (5).jpg" alt="Image 6" class="suggestion">
            <img src="2876053.webp" alt="Image 7" class="suggestion">
        </div>
    </div>

    <!-- Conteneur du film qui s'affiche quand on clique sur "Regarder maintenant" -->
    <div class="movie-container" id="movieContainer">
        <video id="movieVideo" controls class="hidden">
            <source src="DEMON SLAYER Le Film Bande Annonce VF (2021) Le Train de L'Infini.mp4" type="video/mp4">
            Votre navigateur ne supporte pas le format vidéo.
        </video>
        <span class="close-btn" onclick="closeMovie()">X</span>
    </div>

    <script>
        // Ouvre le film au centre
        function openMovie() {
            var movieContainer = document.getElementById('movieContainer');
            var movieVideo = document.getElementById('movieVideo');
            movieContainer.style.display = 'flex';  // Affiche le film
            movieVideo.style.display = 'none';  // Ne pas afficher la vidéo tout de suite
            setTimeout(function() {
                movieVideo.style.display = 'block';  // Affiche la vidéo après 1 seconde
            }, 1000); // Afficher la vidéo après 1 seconde pour un effet
        }

        // Ferme le film
        function closeMovie() {
            var movieContainer = document.getElementById('movieContainer');
            var movieVideo = document.getElementById('movieVideo');
            movieContainer.style.display = 'none';  // Masque le film
            movieVideo.pause();  // Arrête la vidéo
            movieVideo.currentTime = 0;  // Remet la vidéo à zéro
        }

        // Fonction pour ouvrir/fermer la barre de recherche
        function toggleSearch() {
            var searchBar = document.getElementById('searchBar');
            if (searchBar.style.display === 'none' || searchBar.style.display === '') {
                searchBar.style.display = 'block';
            } else {
                searchBar.style.display = 'none';
            }
        }

        // Fonction pour gérer la recherche et faire "monter" les images
        function handleSearchInput() {
            var searchBar = document.getElementById('searchBar').value.toLowerCase();
            var heroSection = document.getElementById('heroSection');
            var imageGallery = document.getElementById('imageGallery');
            var suggestions = document.querySelectorAll('.suggestion');

            if (searchBar.trim() !== '') {
                heroSection.style.opacity = '0';  // Cache l'image en avant-première
                imageGallery.style.transform = 'translateY(-500px)';  // Déplace les images vers le haut
            } else {
                heroSection.style.opacity = '1';  // Affiche l'image en avant-première
                imageGallery.style.transform = 'translateY(0)';  // Remet les images à leur position
            }

            // Masquer les autres suggestions si recherche commence par 'j'
            if (searchBar.startsWith('j')) {
                suggestions.forEach(function(img) {
                    img.classList.add('hidden-suggestions');  // Masquer toutes les images
                });
                var jokerImage = document.querySelector('.joker-suggestion');
                if (jokerImage) {
                    jokerImage.classList.remove('hidden-suggestions');  // Afficher seulement l'image de Joker
                }
            } else if (searchBar.startsWith('d')) {
                suggestions.forEach(function(img) {
                    img.classList.add('hidden-suggestions');  // Masquer toutes les images
                });
                var demonSlayerImage = document.querySelector('.demon-slayer-suggestion');
                if (demonSlayerImage) {
                    demonSlayerImage.classList.remove('hidden-suggestions');  // Afficher seulement l'image de Demon Slayer
                }
            } else {
                // Si ce n'est pas "joker" ou "demon slayer", afficher toutes les images
                suggestions.forEach(function(img) {
                    img.classList.remove('hidden-suggestions');
                });
            }
        }
    </script>
</body>
</html>
