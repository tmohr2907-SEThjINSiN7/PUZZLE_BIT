<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Bitcoin Puzzle Champion - Final Repaired</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #1a1a24;
            color: #ffffff;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            margin: 0;
            padding: 20px;
        }
        h1 { color: #f1c40f; margin-bottom: 5px; font-size: 26px; text-align: center; text-shadow: 0 2px 4px rgba(0,0,0,0.5); }
        
        .crypto-dashboard {
            background: #2c3e50;
            border: 2px solid #f1c40f;
            border-radius: 10px;
            padding: 15px;
            width: 90%;
            max-width: 460px;
            margin-bottom: 20px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.5);
            box-sizing: border-box;
        }
        .balance-title { font-size: 14px; color: #bdc3c7; text-transform: uppercase; letter-spacing: 1px; }
        .balance-btc { font-size: 28px; color: #2ecc71; font-weight: bold; margin: 5px 0; font-family: monospace; }
        
        .wallet-section {
            margin-top: 10px;
            display: flex;
            gap: 5px;
        }
        .wallet-input {
            flex: 1;
            padding: 8px;
            border-radius: 4px;
            border: 1px solid #7f8c8d;
            background: #34495e;
            color: white;
            font-family: monospace;
            font-size: 12px;
        }

        .controls, .filter-section {
            margin-bottom: 15px;
            display: flex;
            gap: 8px;
            flex-wrap: wrap;
            justify-content: center;
            width: 90%;
            max-width: 460px;
        }
        
        select {
            padding: 10px;
            font-size: 14px;
            border-radius: 5px;
            background-color: #34495e;
            color: white;
            border: 1px solid #f1c40f;
            font-weight: bold;
            cursor: pointer;
            width: 100%;
        }

        button, .btn-upload-label {
            padding: 12px 16px;
            font-size: 13px;
            border: none;
            border-radius: 5px;
            background-color: #f1c40f;
            color: #1a1a24;
            cursor: pointer;
            font-weight: bold;
            text-align: center;
            text-decoration: none;
        }
        
        #shuffle-btn { background-color: #f39c12; color: white; }
        #autoplay-btn { background-color: #2ecc71; color: white; }
        #upload-btn-style { background-color: #3498db; color: white; }
        #generate-btn { background-color: #9b59b6; color: white; }
        #save-gallery-btn { background-color: #1abc9c; color: white; }
        #download-btn { background-color: #e67e22; color: white; }

        .file-input-wrapper {
            position: relative;
            overflow: hidden;
            display: inline-block;
        }
        .file-input-wrapper input[type=file] {
            font-size: 100px;
            position: absolute;
            left: 0;
            top: 0;
            opacity: 0;
            cursor: pointer;
        }
        
        #puzzle-container {
            display: grid;
            grid-template-columns: repeat(4, 80px);
            grid-template-rows: repeat(4, 80px);
            gap: 3px;
            background-color: #34495e;
            padding: 6px;
            border-radius: 8px;
            box-shadow: 0 8px 16px rgba(0,0,0,0.6);
            margin-bottom: 20px;
        }
        @media(min-width: 450px) {
            #puzzle-container { grid-template-columns: repeat(4, 100px); grid-template-rows: repeat(4, 100px); }
            .tile { width: 100px !important; height: 100px !important; }
        }
        .tile {
            width: 80px;
            height: 80px;
            cursor: pointer;
            border-radius: 4px;
            box-sizing: border-box;
            background-repeat: no-repeat;
        }
        .tile.selected {
            border: 3px solid #e74c3c;
        }

        .gallery-box {
            width: 90%;
            max-width: 460px;
            background: #2c3e50;
            padding: 15px;
            border-radius: 10px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.5);
            box-sizing: border-box;
        }
        .gallery-title { font-size: 16px; font-weight: bold; color: #f1c40f; margin-bottom: 10px; }
        .gallery-grid {
            display: flex;
            gap: 8px;
            overflow-x: auto;
            padding-bottom: 10px;
            min-height: 70px;
        }
        .gallery-item {
            width: 60px;
            height: 60px;
            border-radius: 5px;
            cursor: pointer;
            border: 2px solid #7f8c8d;
            object-fit: cover;
            flex-shrink: 0;
            background: #1a1a24;
        }
        .gallery-item.active-puzzle { border-color: #2ecc71; box-shadow: 0 0 8px #2ecc71; }

        #status-panel {
            margin-top: 15px;
            padding: 12px;
            background: #000000;
            color: #2ecc71;
            font-family: monospace;
            border-radius: 5px;
            width: 90%;
            max-width: 460px;
            font-size: 11px;
            max-height: 90px;
            overflow-y: auto;
            box-sizing: border-box;
        }
    </style>
</head>
<body>

    <h1>Bitcoin Puzzle Champion</h1>
    
    <div class="crypto-dashboard">
        <div class="balance-title">Dein Wallet-Guthaben:</div>
        <div class="balance-btc" id="wallet-btc">0.00000000 BTC</div>
        
        <div class="wallet-section">
            <input type="text" class="wallet-input" id="wallet-addr" placeholder="Bitcoin Ziel-Adresse">
            <button id="withdraw-btn">Auszahlen</button>
        </div>
    </div>
    
    <div class="controls">
        <button id="shuffle-btn">Mischen</button>
        <button id="autoplay-btn">Autoplay</button>
        <button id="download-btn">Bild Downloaden</button>
    </div>

    <div class="filter-section">
        <select id="image-filter-select">
            <option value="all">Alle Bilder anzeigen</option>
            <option value="default">Standard-Vorlagen</option>
            <option value="user">Eigene Galerie-Bilder</option>
        </select>
        <button id="generate-btn">Zufallsbild erzeugen</button>
        <div class="file-input-wrapper">
            <span class="btn-upload-label" id="upload-btn-style">Upload</span>
            <input type="file" id="image-gallery-uploader" accept="image/*">
        </div>
        <button id="save-gallery-btn">In Galerie speichern</button>
    </div>

    <div id="puzzle-container"></div>

    <div class="gallery-box">
        <div class="gallery-title">Deine Mini-Galerie:</div>
        <div class="gallery-grid" id="gallery-grid"></div>
        <button id="clear-gallery-btn" style="margin-top:10px; background-color:#e74c3c; color:white; padding:5px 10px; font-size:11px;">Galerie leeren</button>
    </div>

    <div id="status-panel">
        <div>[System]: Base64-Injektionsschutz aktiv. Volle Offline-Lauffähigkeit hergestellt.</div>
    </div>

    <script>
        // Wandelt SVGs in saubere Base64 Datenströme um, die niemals CSS crashen können
        function getSvgBase64Url(bgColor, symbol, textLine) {
            var svg = "<svg xmlns='http://www.w3.org/2000/svg' width='400' height='400'>"
                    + "<rect width='100%' height='100%' fill='" + bgColor + "'/>"
                    + "<circle cx='200' cy='200' r='110' fill='none' stroke='white' stroke-width='12' opacity='0.3'/>"
                    + "<text x='200' y='220' font-family='Arial' font-size='120' font-weight='bold' fill='white' text-anchor='middle'>" + symbol + "</text>"
                    + "<text x='200' y='320' font-family='Courier' font-size='20' font-weight='bold' fill='#f1c40f' text-anchor='middle'>" + textLine + "</text>"
                    + "</svg>";
            return "data:image/svg+xml;base64," + btoa(unescape(encodeURIComponent(svg)));
        }

        var DEFAULT_IMAGES = [
            getSvgBase64Url('#f39c12', '₿', 'GENESIS BLOCK'),
            getSvgBase64Url('#1abc9c', '★', 'CHAMPION EDITION')
        ];

        var userImages = [];
        try {
            userImages = JSON.parse(localStorage.getItem('puzzle_user_images')) || [];
        } catch(e) {
            userImages = [];
        }

        var currentImgUrl = DEFAULT_IMAGES[0];

        var container = document.getElementById('puzzle-container');
        var shuffleBtn = document.getElementById('shuffle-btn');
        var autoplayBtn = document.getElementById('autoplay-btn');
        var downloadBtn = document.getElementById('download-btn');
        var generateBtn = document.getElementById('generate-btn');
        var saveGalleryBtn = document.getElementById('save-gallery-btn');
        var imageFilterSelect = document.getElementById('image-filter-select');
        var galleryGrid = document.getElementById('gallery-grid');
        var imageGalleryUploader = document.getElementById('image-gallery-uploader');
        var statusPanel = document.getElementById('status-panel');
        var withdrawBtn = document.getElementById('withdraw-btn');
        var clearGalleryBtn = document.getElementById('clear-gallery-btn');

        var btcBalance = 0.0;
        try {
            btcBalance = parseFloat(localStorage.getItem('puzzle_btc_balance')) || 0.0;
        } catch(e) {
            btcBalance = 0.0;
        }
        
        var gridSize = 4;
        var tiles = [], originalOrder = [], isSolving = false, firstSelectedTile = null;

        document.getElementById('wallet-btc').innerText = btcBalance.toFixed(8) + " BTC";

        function logStatus(msg) {
            var div = document.createElement('div');
            div.innerText = "[System]: " + msg;
            statusPanel.appendChild(div);
            statusPanel.scrollTop = statusPanel.scrollHeight;
        }

        imageFilterSelect.addEventListener('change', renderGallery);

        imageGalleryUploader.addEventListener('change', function(e) {
            if (!e.target.files || !e.target.files[0]) return;
            var reader = new FileReader();
            reader.onload = function(event) {
                currentImgUrl = event.target.result;
                logStatus("Eigenes Bild hochgeladen! Drücke 'In Galerie speichern' zum Behalten.");
                initPuzzle(currentImgUrl);
                renderGallery();
            };
            reader.readAsDataURL(e.target.files[0]);
        });

        generateBtn.addEventListener('click', function() {
            var colors = ['#3498db', '#9b59b6', '#e74c3c', '#1abc9c', '#2c3e50', '#d35400', '#27ae60'];
            var bg = colors[Math.floor(Math.random() * colors.length)];
            var id = Math.floor(Math.random() * 89999 + 10000);
            
            var svg = "<svg xmlns='http://www.w3.org/2000/svg' width='400' height='400'>"
                    + "<rect width='100%' height='100%' fill='" + bg + "'/>"
                    + "<circle cx='200' cy='200' r='130' fill='white' opacity='0.1'/>"
                    + "<text x='200' y='180' font-family='Arial' font-size='36' font-weight='bold' fill='#f1c40f' text-anchor='middle'>NFT ART</text>"
                    + "<text x='200' y='240' font-family='Courier' font-size='40' font-weight='bold' fill='white' text-anchor='middle'>#ID-" + id + "</text>"
                    + "</svg>";

            currentImgUrl = "data:image/svg+xml;base64," + btoa(unescape(encodeURIComponent(svg)));
            logStatus("Zufallsbild erfolgreich generiert!");
            initPuzzle(currentImgUrl);
            renderGallery();
        });

        saveGalleryBtn.addEventListener('click', function() {
            if (userImages.indexOf(currentImgUrl) !== -1 || DEFAULT_IMAGES.indexOf(currentImgUrl) !== -1) {
                logStatus("Bild ist bereits vorhanden.");
                return;
            }
            if(userImages.length >= 8) {
                alert("Galerie voll (max. 8 eigene Bilder). Bitte leere deine Galerie zuerst.");
                return;
            }
            userImages.push(currentImgUrl);
            try {
                localStorage.setItem('puzzle_user_images', JSON.stringify(userImages));
                logStatus("Bild erfolgreich in Galerie gesichert!");
                renderGallery();
            } catch(e) {
                alert("Fehler beim Speichern!");
            }
        });

        downloadBtn.addEventListener('click', function() {
            var link = document.createElement('a');
            link.download = "puzzle-art-" + Date.now() + (currentImgUrl.indexOf("png") !== -1 || currentImgUrl.indexOf("jpeg") !== -1 ? ".jpg" : ".svg");
            link.href = currentImgUrl;
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            logStatus("Bild-Download gestartet.");
        });

        function renderGallery() {
            galleryGrid.innerHTML = '';
            var filter = imageFilterSelect.value;
            var showList = [];

            if (filter === 'all') showList = showList.concat(DEFAULT_IMAGES, userImages);
            else if (filter === 'default') showList = showList.concat(DEFAULT_IMAGES);
            else if (filter === 'user') showList = showList.concat(userImages);

            for (var i = 0; i < showList.length; i++) {
                (function(imgUrl) {
                    var img = document.createElement('img');
                    img.src = imgUrl;
                    img.classList.add('gallery-item');
                    if (imgUrl === currentImgUrl) img.classList.add('active-puzzle');
                    
                    img.addEventListener('click', function() {
                        if (isSolving) return;
                        currentImgUrl = imgUrl;
                        initPuzzle(currentImgUrl);
                        renderGallery();
                    });
                    galleryGrid.appendChild(img);
                })(showList[i]);
            }
        }

        function initPuzzle(imgUrl) {
            container.innerHTML = '';
            tiles = []; originalOrder = []; firstSelectedTile = null;
            var isMobile = window.innerWidth < 450;
            var totalSize = isMobile ? 320 : 400;

            for (var i = 0; i < gridSize * gridSize; i++) {
                var tile = document.createElement('div');
                tile.classList.add('tile');
                // HIER DER FIX: Nutzung von doppelten Anführungszeichen, um CSS-Konflikte mit Base64-Rohdaten zu vermeiden
                tile.style.backgroundImage = 'url("' + imgUrl + '")';
                tile.style.backgroundSize = totalSize + "px " + totalSize + "px";
                
                var row = Math.floor(i / gridSize);
                var col = i % gridSize;
                tile.style.backgroundPosition = ((col / 3) * 100) + "% " + ((row / 3) * 100) + "%";
                tile.dataset.correctIndex = i;
                
                tile.addEventListener('click', (function(t) {
                    return function() { handleTileClick(t); };
                })(tile));
                
                tiles.push(tile);
                originalOrder.push(tile);
                container.appendChild(tile);
            }
        }

        function handleTileClick(clickedTile) {
            if (isSolving) return;

            if (!firstSelectedTile) {
                firstSelectedTile = clickedTile;
                firstSelectedTile.classList.add('selected');
            } else if (firstSelectedTile === clickedTile) {
                firstSelectedTile.classList.remove('selected');
                firstSelectedTile = null;
            } else {
                firstSelectedTile.classList.remove('selected');
                var idx1 = tiles.indexOf(firstSelectedTile);
                var idx2 = tiles.indexOf(clickedTile);
                
                var temp = tiles[idx1];
                tiles[idx1] = tiles[idx2];
                tiles[idx2] = temp;
                
                container.innerHTML = '';
                for (var i = 0; i < tiles.length; i++) {
                    container.appendChild(tiles[i]);
                }
                
                firstSelectedTile = null;
                
                var isWon = true;
                for (var j = 0; j < tiles.length; j++) {
                    if (parseInt(tiles[j].dataset.correctIndex) !== j) {
                        isWon = false;
                    }
                }

                if(isWon) {
                    logStatus("Gewonnen! +0.00025000 BTC erhalten.");
                    btcBalance += 0.00025000;
                    try { localStorage.setItem('puzzle_btc_balance', btcBalance); } catch(e){}
                    document.getElementById('wallet-btc').innerText = btcBalance.toFixed(8) + " BTC";
                }
            }
        }

        shuffleBtn.addEventListener('click', function() {
            if (isSolving) return;
            for (var i = tiles.length - 1; i > 0; i--) {
                var j = Math.floor(Math.random() * (i + 1));
                var temp = tiles[i];
                tiles[i] = tiles[j];
                tiles[j] = temp;
            }
            container.innerHTML = '';
            for (var k = 0; k < tiles.length; k++) {
                container.appendChild(tiles[k]);
            }
            logStatus("Spielfeld gemischt.");
        });

        autoplayBtn.addEventListener('click', function() {
            if(isSolving) return;
            logStatus("Autoplay läuft...");
            isSolving = true;
            setTimeout(function() {
                tiles = [].concat(originalOrder);
                container.innerHTML = '';
                for (var k = 0; k < tiles.length; k++) {
                    container.appendChild(tiles[k]);
                }
                isSolving = false;
                logStatus("Gelöst.");
            }, 600);
        });

        clearGalleryBtn.addEventListener('click', function() {
            if(confirm("Eigene Bilder komplett löschen?")) {
                try { localStorage.removeItem('puzzle_user_images'); } catch(e){}
                userImages = [];
                currentImgUrl = DEFAULT_IMAGES[0];
                renderGallery();
                initPuzzle(currentImgUrl);
                logStatus("Eigene Galerie geleert.");
            }
        });

        withdrawBtn.addEventListener('click', function() {
            var addr = document.getElementById('wallet-addr').value;
            if(!addr || btcBalance <= 0) { alert("Fehler: Keine Adresse oder kein Guthaben."); return; }
            alert("Auszahlung beantragt.");
            btcBalance = 0;
            try { localStorage.setItem('puzzle_btc_balance', btcBalance); } catch(e){}
            document.getElementById('wallet-btc').innerText = "0.00000000 BTC";
        });

        initPuzzle(currentImgUrl);
        renderGallery();
    </script>
</body>
</html>
