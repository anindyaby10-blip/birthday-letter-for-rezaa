<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>A Gift for Reza</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Dancing+Script:wght@700&family=Playfair+Display:ital,wght@0,400;0,600;1,400&family=Great+Vibes&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg-color: #fdfbf7;
            --env-color: #eaddcf;
            --env-border: #c4b5a3;
            --text-color: #3d352b;
            --wax-red: #9e2b2b;
        }
        * { box-sizing: border-box; margin: 0; padding: 0; }
        body {
            background-color: var(--bg-color);
            background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noiseFilter'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.65' numOctaves='3' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noiseFilter)' opacity='0.05'/%3E%3C/svg%3E");
            font-family: 'Playfair Display', serif;
            color: var(--text-color);
            height: 100vh;
            overflow-x: hidden;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            position: relative;
        }

        #screen1, #screen2 { text-align: center; }
        #screen1 { cursor: pointer; animation: fadeIn 1s ease; }
        #screen1 h1 { font-family: 'Great Vibes', cursive; font-size: 36px; }
        #screen1 p { font-size: 14px; opacity: 0.6; margin-top: 20px; }

        #screen2 { display: none; animation: fadeIn 0.5s ease; }
        #screen2 h2 { font-family: 'Dancing Script', cursive; font-size: 28px; margin-bottom: 30px; }
        .btn-container { display: flex; gap: 30px; position: relative; width: 220px; }
        .btn { padding: 14px 36px; border: none; border-radius: 30px; font-family: 'Playfair Display', serif; font-size: 16px; cursor: pointer; }
        .btn-yes { background: var(--text-color); color: white; }
        .btn-yes:hover { transform: scale(1.1); }
        .btn-no { background: #ccc; color: #666; position: absolute; left: 120px; }
        .shake { animation: shake 0.5s ease infinite; }

        #screen3 { display: none; width: 100%; max-width: 500px; padding: 20px; }
        #screen3.show { display: block; }

        #screen4 { 
            display: none; 
            position: fixed;
            top: 0; left: 0;
            width: 100%; height: 100%;
            background: var(--bg-color);
            z-index: 200;
            justify-content: center;
            align-items: center;
            flex-direction: column;
            text-align: center;
        }
        #screen4 h1 { font-family: 'Great Vibes', cursive; font-size: 32px; }
        #screen4 p { margin-top: 15px; opacity: 0.6; }

        #sparkles { position: fixed; top: 0; left: 0; width: 100%; height: 100%; pointer-events: none; z-index: 100; }
        .sparkle { position: absolute; background: white; border-radius: 50%; opacity: 0; animation: twinkle 2s infinite; }

        .envelope-container {
            position: relative; width: 400px; height: 280px;
            perspective: 1200px; cursor: pointer;
            transition: transform 0.5s ease;
        }
        .envelope-container:hover { transform: translateY(-8px); }

        .envelope-back {
            position: absolute; width: 100%; height: 100%;
            background: var(--env-color); border-radius: 10px;
            box-shadow: 0 8px 25px rgba(0,0,0,0.15);
            border: 2px solid var(--env-border);
            transition: all 0.8s ease;
        }
        .envelope-back::before {
            content: ''; position: absolute; top: 10px; left: 10px; right: 10px; bottom: 10px;
            border: 1px dashed var(--env-border); border-radius: 6px; opacity: 0.4;
        }

        .envelope-flap {
            position: absolute; top: 0; left: 0;
            width: 100%; height: 130px;
            background: var(--env-color);
            clip-path: polygon(0 0, 50% 100%, 100% 0);
            transform-origin: top;
            transition: all 0.7s ease;
            z-index: 5;
            border-top-left-radius: 10px; border-top-right-radius: 10px;
            border: 2px solid var(--env-border);
        }

        .envelope-front {
            position: absolute; bottom: 0; left: 0;
            width: 100%; height: 150px;
            background: linear-gradient(135deg, var(--env-color) 0%, #dccfb8 100%);
            z-index: 6;
            border-bottom-left-radius: 10px; border-bottom-right-radius: 10px;
            border: 2px solid var(--env-border);
            transition: all 0.8s ease;
        }

        .letter {
            position: absolute;
            width: 340px; height: 460px;
            background: #faf8f2;
            left: 30px; top: 30px;
            padding: 50px 40px;
            box-shadow: 0 4px 20px rgba(0,0,0,0.12);
            transform: translateY(0);
            transform-origin: top center;
            transition: all 0.9s ease;
            opacity: 0;
            z-index: 1;
            pointer-events: none;
            overflow-y: auto;
            font-size: 14px;
            line-height: 1.9;
            border: 1px solid #e8e4db;
        }

        .letter-content { opacity: 0; transition: opacity 1s ease 0.6s; transform: translateY(20px); }

        .close-btn {
            display: none;
            margin-top: 30px;
            padding: 12px 30px;
            background: var(--text-color);
            color: white;
            border: none;
            border-radius: 25px;
            font-family: 'Playfair Display', serif;
            font-size: 14px;
            cursor: pointer;
        }
        .letter .close-btn { display: block; opacity: 0; animation: fadeIn 0.5s ease 0.5s forwards; }

        .wax-seal {
            position: absolute;
            width: 50px; height: 50px;
            background: var(--wax-red);
            border-radius: 50%;
            top: 60%; left: 50%;
            transform: translate(-50%, -50%);
            box-shadow: inset 0 0 12px rgba(0,0,0,0.4);
            z-index: 20;
            display: flex; align-items: center; justify-content: center;
        }
        .wax-seal::after { content: "R"; color: rgba(255,255,255,0.35); font-family: 'Great Vibes', cursive; font-size: 28px; }

        /* Open Animation */
        .envelope-container.open .wax-seal { top: 10%; left: 90%; transform: translate(-50%, -50%) rotate(45deg); opacity: 0.1; width: 30px; height: 30px; }
        .envelope-container.open .envelope-flap { transform: rotateX(180deg); opacity: 0; }
        .envelope-container.open .letter { transform: translateY(-200px) scale(1.15); opacity: 1; z-index: 2; pointer-events: auto; }
        .envelope-container.open .letter-content { opacity: 1; transform: translateY(0); }
        .envelope-container.open .envelope-back { transform: translateY(120px); opacity: 0; }
        .envelope-container.open .envelope-front { transform: translateY(180px); opacity: 0; }

        h1.letter-title { font-family: 'Great Vibes', cursive; font-size: 32px; margin-bottom: 15px; text-align: center; }
        h2.letter-subtitle { font-family: 'Dancing Script', cursive; font-size: 18px; text-align: center; margin-bottom: 25px; opacity: 0.8; }
        p.letter-text { margin-bottom: 18px; text-align: justify; font-size: 13.5px; }
        .signature { margin-top: 40px; text-align: right; font-family: 'Great Vibes', cursive; font-size: 26px; }

        @keyframes fadeIn { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        @keyframes shake { 0%, 100% { transform: translateX(0); } 25% { transform: translateX(-15px); } 75% { transform: translateX(15px); } }
        @keyframes twinkle { 0% { transform: scale(0); opacity: 0; } 50% { transform: scale(1); opacity: 0.8; } 100% { transform: scale(0); opacity: 0; } }

        @media (max-width: 450px) {
            .letter { width: 300px; left: 15px; padding: 35px 25px; }
            .envelope-container { width: 330px; height: 230px; }
            .envelope-container.open .letter { transform: translateY(-180px) scale(1.1); }
        }
    </style>
</head>
<body>
    <div id="sparkles"></div>

    <div id="screen1" onclick="goToScreen2()">
        <h1>A gift for Reza</h1>
        <p>click to open</p>
    </div>

    <div id="screen2">
        <h2>Want to open?</h2>
        <div class="btn-container">
            <button class="btn btn-yes" onclick="chooseYes()">Yes</button>
            <button class="btn btn-no" id="btnNo" onmouseover="moveNo()" onclick="moveNo()">No</button>
        </div>
    </div>

    <div id="screen3">
        <div class="envelope-container" id="envelope" onclick="openEnvelope()">
            <div class="envelope-back"></div>
            <div class="letter">
                <div class="letter-content">
                    <h1 class="letter-title">Happy Birthday, sayang</h1>
                    <h2 class="letter-subtitle">Sebuah surat untukmu</h2>
                    <p class="letter-text">Aku sebenarnya bukan orang yang pandai bikin birthday letter yang penuh kata-kata manis. Tapi di hari ini, aku cuma ingin mengingatkan beberapa hal yang mungkin jarang kamu dengar.</p>
                    <p class="letter-text">Pertama, aku harap kamu berhenti terlalu keras pada dirimu sendiri. Kamu sering fokus pada hal-hal yang belum tercapai sampai lupa melihat sejauh apa kamu sudah berjalan. Kalau aku melihatmu, yang aku lihat bukan seseorang yang kurang. Aku melihat seseorang yang terus berusaha, terus belajar, dan terus bertahan meskipun tidak selalu mudah.</p>
                    <p class="letter-text">Thank you for being the person that you are. Not the perfect version of you, but the real one. The one who has flaws, bad days, random worries, and dreams that they are still chasing. I think that is the version of you that deserves to be loved the most.</p>
                    <p class="letter-text">Di umur yang baru ini, aku tidak berharap hidupmu selalu bahagia. Karena hidup memang tidak bekerja seperti itu. Tapi aku berharap kamu punya lebih banyak alasan untuk tersenyum, lebih banyak ketenangan, dan lebih sedikit hal yang membuatmu meragukan dirimu sendiri.</p>
                    <p class="letter-text">I hope this year feels lighter. I hope you find yourself worrying less and living more. And I hope that whenever things get difficult, you will remember that you do not have to carry everything alone.</p>
                    <p class="letter-text">Terima kasih karena sudah menjadi bagian dari hidupku. Terima kasih untuk semua obrolan, perhatian, dukungan, dan hal-hal kecil yang mungkin terlihat biasa, tapi sangat berarti buat aku.</p>
                    <p class="letter-text">Hari ini adalah hari tentang kamu. Jadi sebelum hari ini berakhir, please take a moment to appreciate yourself. You have made it this far, and that is something worth celebrating.</p>
                    <p class="letter-text">Happy birthday, my love.</p>
                    <p class="letter-text">Semoga tahun ini memperlakukanmu dengan baik, dan semoga semua hal baik yang kamu berikan ke orang lain, kembali ke kamu dalam bentuk yang lebih besar.</p>
                    <div class="signature">With love,<br>Anindya</div>
                    <button class="close-btn" onclick="closeLetter()">Close</button>
                </div>
            </div>
            <div class="envelope-flap"></div>
            <div class="envelope-front"></div>
            <div class="wax-seal"></div>
        </div>
    </div>

    <div id="screen4">
        <h1>Thank you for reading this</h1>
        <p>Hope you have a wonderful day</p>
    </div>

    <script>
        // Sparkles
        var container = document.getElementById('sparkles');
        for (var i = 0; i < 25; i++) {
            var sparkle = document.createElement('div');
            sparkle.className = 'sparkle';
            sparkle.style.left = Math.random() * 100 + '%';
            sparkle.style.top = Math.random() * 100 + '%';
            sparkle.style.width = (Math.random() * 4 + 2) + 'px';
            sparkle.style.height = sparkle.style.width;
            sparkle.style.animationDelay = Math.random() * 2 + 's';
            container.appendChild(sparkle);
        }

        // Functions
        function goToScreen2() {
            document.getElementById('screen1').style.display = 'none';
            document.getElementById('screen2').style.display = 'flex';
        }

        function moveNo() {
            var btnNo = document.getElementById('btnNo');
            var randomX = Math.random() * 160 - 80;
            var randomY = Math.random() * 120 - 60;
            btnNo.style.transform = 'translate(' + randomX + 'px, ' + randomY + 'px)';
            btnNo.classList.add('shake');
        }

        function chooseYes() {
            document.getElementById('screen2').style.display = 'none';
            var screen3 = document.getElementById('screen3');
            screen3.style.display = 'block';
            setTimeout(function() {
                screen3.classList.add('show');
            }, 100);
            setTimeout(function() {
                document.getElementById('envelope').classList.add('open');
            }, 900);
        }

        function openEnvelope() {
            document.getElementById('envelope').classList.add('open');
        }

        function closeLetter() {
            document.getElementById('screen4').style.display = 'flex';
        }
    </script>
</body>
</html>
