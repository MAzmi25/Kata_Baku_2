<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [


    { "en": "Kata Baku Dari 'Kaluarga'?", "id": "Keluarga" },
    { "en": "Kata Baku Dari 'Diluar'?", "id": "Di Luar" },
    { "en": "Kata Baku Dari 'Kembangin'?", "id": "Mengembangkan" },
    { "en": "Kata Baku Dari 'Menyan'?", "id": "Kemenyan" },
    { "en": "Kata Baku Dari 'Kemerah Merahan'?", "id": "Kemerah-merahan" },
    { "en": "Kata Baku Dari 'Kenang Kenangan'?", "id": "Kenang-kenangan" },
    { "en": "Kata Baku Dari 'Kencing'?", "id": "Buang Air Kecil" },
    { "en": "Kata Baku Dari 'Kepada Ku'?", "id": "Kepadaku" },
    { "en": "Kata Baku Dari 'Kepada Mu'?", "id": "Kepadamu" },
    { "en": "Kata Baku Dari 'Kepala Nya'?", "id": "Kepalanya" },
    { "en": "Kata Baku Dari 'Kepingin'?", "id": "Ingin" },
    { "en": "Kata Baku Dari 'Ponakan'?", "id": "Keponakan" },
    { "en": "Kata Baku Dari 'Kramas'?", "id": "Keramas" },
    { "en": "Kata Baku Dari 'Kranjang'?", "id": "Keranjang" },
    { "en": "Kata Baku Dari 'Kebo'?", "id": "Kerbau" },
    { "en": "Kata Baku Dari 'Kerjasama'?", "id": "Kerja Sama" },
    { "en": "Kata Baku Dari 'Kerkop'?", "id": "Kuburan" },
    { "en": "Kata Baku Dari 'Kermi'?", "id": "Keremi" },
    { "en": "Kata Baku Dari 'Kroncong'?", "id": "Keroncong" },
    { "en": "Kata Baku Dari 'Ngerumun'?", "id": "Berkerumun" },
    { "en": "Kata Baku Dari 'Krupuk'?", "id": "Kerupuk" },
    { "en": "Kata Baku Dari 'Kesasar'?", "id": "Tersesat" },
    { "en": "Kata Baku Dari 'Kesohor'?", "id": "Mashur" },
    { "en": "Kata Baku Dari 'Ketawa'?", "id": "Tertawa" },
    { "en": "Kata Baku Dari 'Ketela'?", "id": "Ubi" },
    { "en": "Kata Baku Dari 'Ketek'?", "id": "Ketiak" },
    { "en": "Kata Baku Dari 'Ketiban'?", "id": "Tertimpa" },
    { "en": "Kata Baku Dari 'Ketok'?", "id": "Ketuk" },
    { "en": "Kata Baku Dari 'Khasanah'?", "id": "Khazanah" },
    { "en": "Kata Baku Dari 'Hianat'?", "id": "Khianat" },
    { "en": "Kata Baku Dari 'Khusuk'?", "id": "Khusyuk" },
    { "en": "Kata Baku Dari 'Kibul'?", "id": "Bohong" },
    { "en": "Kata Baku Dari 'Ngirim'?", "id": "Mengirim" },
    { "en": "Kata Baku Dari 'Kisi Kisi'?", "id": "Kisi-kisi" },
    { "en": "Kata Baku Dari 'Kocar Kacir'?", "id": "Kocar-kacir" },
    { "en": "Kata Baku Dari 'Kocek'?", "id": "Saku" },
    { "en": "Kata Baku Dari 'Koir'?", "id": "Paduan Suara" },
    { "en": "Kata Baku Dari 'Koit'?", "id": "Mati" },
    { "en": "Kata Baku Dari 'Kokes'?", "id": "Kokas" },
    { "en": "Kata Baku Dari 'Komat Kamit'?", "id": "Komat-kamit" },
    { "en": "Kata Baku Dari 'Komplain'?", "id": "Keluhan" },
    { "en": "Kata Baku Dari 'Komposer'?", "id": "Komponis" },
    { "en": "Kata Baku Dari 'Kenek'?", "id": "Kondektur" },
    { "en": "Kata Baku Dari 'Konfrontir'?", "id": "Konfrontasi" },
    { "en": "Kata Baku Dari 'Koneksi'?", "id": "Koneksi" },
    { "en": "Kata Baku Dari 'Konfeksi'?", "id": "Konfeksi" },
    { "en": "Kata Baku Dari 'Kongres'?", "id": "Kongres" },
    { "en": "Kata Baku Dari 'Konotasi'?", "id": "Konotasi" },
    { "en": "Kata Baku Dari 'Konsep'?", "id": "Konsep" },
    { "en": "Kata Baku Dari 'Konservasi'?", "id": "Konservasi" },
    { "en": "Kata Baku Dari 'Konsisten'?", "id": "Konsisten" },
    { "en": "Kata Baku Dari 'Konstitusi'?", "id": "Konstitusi" },
    { "en": "Kata Baku Dari 'Konstruksi'?", "id": "Konstruksi" },
    { "en": "Kata Baku Dari 'Konsul'?", "id": "Konsul" },
    { "en": "Kata Baku Dari 'Konsumen'?", "id": "Konsumen" },
    { "en": "Kata Baku Dari 'Kontak'?", "id": "Kontak" },
    { "en": "Kata Baku Dari 'Kontan'?", "id": "Kontan" },
    { "en": "Kata Baku Dari 'Konteks'?", "id": "Konteks" },
    { "en": "Kata Baku Dari 'Kontestan'?", "id": "Kontestan" },
    { "en": "Kata Baku Dari 'Kontinen'?", "id": "Benua" },
    { "en": "Kata Baku Dari 'Kontra'?", "id": "Kontra" },
    { "en": "Kata Baku Dari 'Kontrak'?", "id": "Kontrak" },
    { "en": "Kata Baku Dari 'Kontrol'?", "id": "Kontrol" },
    { "en": "Kata Baku Dari 'Kontur'?", "id": "Kontur" },
    { "en": "Kata Baku Dari 'Konvensi'?", "id": "Konvensi" },
    { "en": "Kata Baku Dari 'Konvoi'?", "id": "Konvoi" },
    { "en": "Kata Baku Dari 'Koperasi'?", "id": "Koperasi" },
    { "en": "Kata Baku Dari 'Koper'?", "id": "Koper" },
    { "en": "Kata Baku Dari 'Kopiah'?", "id": "Kopiah" },
    { "en": "Kata Baku Dari 'Kopling'?", "id": "Kopling" },
    { "en": "Kata Baku Dari 'Koral'?", "id": "Koral" },
    { "en": "Kata Baku Dari 'Korban'?", "id": "Korban" },
    { "en": "Kata Baku Dari 'Korden'?", "id": "Gorden" },
    { "en": "Kata Baku Dari 'Korek'?", "id": "Korek" },
    { "en": "Kata Baku Dari 'Koreksi'?", "id": "Koreksi" },
    { "en": "Kata Baku Dari 'Koren'?", "id": "Goren" },
    { "en": "Kata Baku Dari 'Korupsi'?", "id": "Korupsi" },
    { "en": "Kata Baku Dari 'Korvet'?", "id": "Korvet" },
    { "en": "Kata Baku Dari 'Kosakata'?", "id": "Kosakata" },
    { "en": "Kata Baku Dari 'Kostum'?", "id": "Kostum" },
    { "en": "Kata Baku Dari 'Kota'?", "id": "Kota" },
    { "en": "Kata Baku Dari 'Kotak'?", "id": "Kotak" },
    { "en": "Kata Baku Dari 'Kotor'?", "id": "Kotor" },
    { "en": "Kata Baku Dari 'Koyak'?", "id": "Koyak" },
    { "en": "Kata Baku Dari 'Kuesionèr'?", "id": "Kuesioner" },
    { "en": "Kata Baku Dari 'Kraton'?", "id": "Keraton" },
    { "en": "Kata Baku Dari 'Kremasi'?", "id": "Kremasi" },
    { "en": "Kata Baku Dari 'Kriminal'?", "id": "Kriminal" },
    { "en": "Kata Baku Dari 'Krisis'?", "id": "Krisis" },
    { "en": "Kata Baku Dari 'Kristen'?", "id": "Kristen" },
    { "en": "Kata Baku Dari 'Kriteria'?", "id": "Kriteria" },
    { "en": "Kata Baku Dari 'Kritik'?", "id": "Kritik" },
    { "en": "Kata Baku Dari 'Kromosom'?", "id": "Kromosom" },
    { "en": "Kata Baku Dari 'Kronis'?", "id": "Kronis" },
    { "en": "Kata Baku Dari 'Kronologi'?", "id": "Kronologi" },
    { "en": "Kata Baku Dari 'Kuba'?", "id": "Kubah" },
    { "en": "Kata Baku Dari 'Kuini'?", "id": "Kuweni" },
    { "en": "Kata Baku Dari 'Kokoh'?", "id": "Kukuh" },
    { "en": "Kata Baku Dari 'Kul'?", "id": "Kuliah" },
    { "en": "Kata Baku Dari 'Kumpul Kebo'?", "id": "Kumpul Kebo" },
    { "en": "Kata Baku Dari 'Kunang Kunang'?", "id": "Kunang-kunang" },
    { "en": "Kata Baku Dari 'Koneng'?", "id": "Kuning" },
    { "en": "Kata Baku Dari 'Ngupas'?", "id": "Mengupas" },
    { "en": "Kata Baku Dari 'Kupu Kupu'?", "id": "Kupu-kupu" },
    { "en": "Kata Baku Dari 'Kuping'?", "id": "Telinga" },
    { "en": "Kata Baku Dari 'Lab'?", "id": "Laboratorium" },
    { "en": "Kata Baku Dari 'Laba Laba'?", "id": "Laba-laba" },
    { "en": "Kata Baku Dari 'Lebel'?", "id": "Label" },
    { "en": "Kata Baku Dari 'Meladeni'?", "id": "Melayani" },
    { "en": "Kata Baku Dari 'Legislatip'?", "id": "Legislatif" },
    { "en": "Kata Baku Dari 'Lengket'?", "id": "Lekat" },
    { "en": "Kata Baku Dari 'Guyon'?", "id": "Lelucon" },
    { "en": "Kata Baku Dari 'Lepas Landas'?", "id": "Lepas Landas" },
    { "en": "Kata Baku Dari 'Lip'?", "id": "Lift" },
    { "en": "Kata Baku Dari 'Liat'?", "id": "Lihat" },
    { "en": "Kata Baku Dari 'Likwidasi'?", "id": "Likuidasi" },
    { "en": "Kata Baku Dari 'Limoen'?", "id": "Limun" },
    { "en": "Kata Baku Dari 'Gilas'?", "id": "Lindas" },
    { "en": "Kata Baku Dari 'Lini Masa'?", "id": "Linimasa" },
    { "en": "Kata Baku Dari 'Lipen'?", "id": "Lipstik" },
    { "en": "Kata Baku Dari 'Logica'?", "id": "Logika" },
    { "en": "Kata Baku Dari 'Lombok'?", "id": "Cabai" },
    { "en": "Kata Baku Dari 'Luarbiasa'?", "id": "Luar Biasa" },
    { "en": "Kata Baku Dari 'Luks'?", "id": "Mewah" },
    { "en": "Kata Baku Dari 'Maag'?", "id": "Mag" },
    { "en": "Kata Baku Dari 'Madukara'?", "id": "Madukara" },
    { "en": "Kata Baku Dari 'Mafia'?", "id": "Mafia" },
    { "en": "Kata Baku Dari 'Magnet'?", "id": "Magnet" },
    { "en": "Kata Baku Dari 'Mahir'?", "id": "Mahir" },
    { "en": "Kata Baku Dari 'Mahkamah'?", "id": "Mahkamah" },
    { "en": "Kata Baku Dari 'Mahkota'?", "id": "Mahkota" },
    { "en": "Kata Baku Dari 'Mahligai'?", "id": "Mahligai" },
    { "en": "Kata Baku Dari 'Mahluk'?", "id": "Makhluk" },
    { "en": "Kata Baku Dari 'Main'?", "id": "Main" },
    { "en": "Kata Baku Dari 'Majalah'?", "id": "Majalah" },
    { "en": "Kata Baku Dari 'Majelis'?", "id": "Majelis" },
    { "en": "Kata Baku Dari 'Majikan'?", "id": "Majikan" },
    { "en": "Kata Baku Dari 'Maju'?", "id": "Maju" },
    { "en": "Kata Baku Dari 'Maka'?", "id": "Maka" },
    { "en": "Kata Baku Dari 'Makalah'?", "id": "Makalah" },
    { "en": "Kata Baku Dari 'Makam'?", "id": "Makam" },
    { "en": "Kata Baku Dari 'Makan'?", "id": "Makan" },
    { "en": "Kata Baku Dari 'Makar'?", "id": "Makar" },
    { "en": "Kata Baku Dari 'Makin'?", "id": "Makin" },
    { "en": "Kata Baku Dari 'Maklum'?", "id": "Maklum" },
    { "en": "Kata Baku Dari 'Makmur'?", "id": "Makmur" },
    { "en": "Kata Baku Dari 'Makna'?", "id": "Makna" },
    { "en": "Kata Baku Dari 'Makro'?", "id": "Makro" },
    { "en": "Kata Baku Dari 'Maksiat'?", "id": "Maksiat" },
    { "en": "Kata Baku Dari 'Maksud'?", "id": "Maksud" },
    { "en": "Kata Baku Dari 'Mala'?", "id": "Mala" },
    { "en": "Kata Baku Dari 'Malam'?", "id": "Malam" },
    { "en": "Kata Baku Dari 'Malang'?", "id": "Malang" },
    { "en": "Kata Baku Dari 'Malaria'?", "id": "Malaria" },
    { "en": "Kata Baku Dari 'Malas'?", "id": "Malas" },
    { "en": "Kata Baku Dari 'Malu'?", "id": "Malu" },
    { "en": "Kata Baku Dari 'Mama'?", "id": "Ibu" },
    { "en": "Kata Baku Dari 'Mamah'?", "id": "Memamah" },
    { "en": "Kata Baku Dari 'Mamalia'?", "id": "Mamalia" },
    { "en": "Kata Baku Dari 'Mampir'?", "id": "Mampir" },
    { "en": "Kata Baku Dari 'Mampu'?", "id": "Mampu" },
    { "en": "Kata Baku Dari 'Manajer'?", "id": "Manajer" },
    { "en": "Kata Baku Dari 'Manasuka'?", "id": "Manasuka" },
    { "en": "Kata Baku Dari 'Mancanegara'?", "id": "Mancanegara" },
    { "en": "Kata Baku Dari 'Mancung'?", "id": "Mancung" },
    { "en": "Kata Baku Dari 'Mandi'?", "id": "Mandi" },
    { "en": "Kata Baku Dari 'Mandiri'?", "id": "Mandiri" },
    { "en": "Kata Baku Dari 'Mandor'?", "id": "Mandor" },
    { "en": "Kata Baku Dari 'Manekin'?", "id": "Maneken" },
    { "en": "Kata Baku Dari 'Mangga'?", "id": "Mangga" },
    { "en": "Kata Baku Dari 'Mangkir'?", "id": "Mangkir" },
    { "en": "Kata Baku Dari 'Maniak'?", "id": "Maniak" },
    { "en": "Kata Baku Dari 'Manifes'?", "id": "Manifes" },
    { "en": "Kata Baku Dari 'Manis'?", "id": "Manis" },
    { "en": "Kata Baku Dari 'Manja'?", "id": "Manja" },
    { "en": "Kata Baku Dari 'Mantan'?", "id": "Mantan" },
    { "en": "Kata Baku Dari 'Mantap'?", "id": "Mantap" },
    { "en": "Kata Baku Dari 'Manual'?", "id": "Manual" },
    { "en": "Kata Baku Dari 'Manusia'?", "id": "Manusia" },
    { "en": "Kata Baku Dari 'Map'?", "id": "Map" },
    { "en": "Kata Baku Dari 'Mapan'?", "id": "Mapan" },
    { "en": "Kata Baku Dari 'Marah'?", "id": "Marah" },
    { "en": "Kata Baku Dari 'Maret'?", "id": "Maret" },
    { "en": "Kata Baku Dari 'Mempesona'?", "id": "Memesona" },
    { "en": "Kata Baku Dari 'Mentackle'?", "id": "Menekel" },
    { "en": "Kata Baku Dari 'Mensuplai'?", "id": "Menyuplai" },
    { "en": "Kata Baku Dari 'Menterjemahkan'?", "id": "Menerjemahkan" },
    { "en": "Kata Baku Dari 'Mengkarantina'?", "id": "Mengarantina" },
    { "en": "Kata Baku Dari 'Pengambilalihan'?", "id": "Pengambilalihan" },
    { "en": "Kata Baku Dari 'Marmar'?", "id": "Marmer" },
    { "en": "Kata Baku Dari 'Problem'?", "id": "Masalah" },
    { "en": "Kata Baku Dari 'Masi'?", "id": "Masih" },
    { "en": "Kata Baku Dari 'Masing Masing'?", "id": "Masing-masing" },
    { "en": "Kata Baku Dari 'Masukakal'?", "id": "Masuk Akal" },
    { "en": "Kata Baku Dari 'Masarakat'?", "id": "Masyarakat" },
    { "en": "Kata Baku Dari 'Mata Mata'?", "id": "Mata-mata" },
    { "en": "Kata Baku Dari 'Matapelajaran'?", "id": "Mata Pelajaran" },
    { "en": "Kata Baku Dari 'Materil'?", "id": "Materiil" },
    { "en": "Kata Baku Dari 'Mo'?", "id": "Mau" },
    { "en": "Kata Baku Dari 'Majoritas'?", "id": "Mayoritas" },
    { "en": "Kata Baku Dari 'Mayonaise'?", "id": "Mayones" },
    { "en": "Kata Baku Dari 'Memory'?", "id": "Memori" },
    { "en": "Kata Baku Dari 'Alm.'?", "id": "Mendiang" },
    { "en": "Kata Baku Dari 'Mending'?", "id": "Lebih Baik" },
    { "en": "Kata Baku Dari 'Mens'?", "id": "Haid" },
    { "en": "Kata Baku Dari 'Musti'?", "id": "Mesti" },
    { "en": "Kata Baku Dari 'Matic'?", "id": "Metik" },
    { "en": "Kata Baku Dari 'Mikro Bus'?", "id": "Mikrobus" },
    { "en": "Kata Baku Dari 'Mistery'?", "id": "Misteri" },
    { "en": "Kata Baku Dari 'Mixer'?", "id": "Mikser" },
    { "en": "Kata Baku Dari 'Moga'?", "id": "Semoga" },
    { "en": "Kata Baku Dari 'Mondar Mandir'?", "id": "Mondar-mandir" },
    { "en": "Kata Baku Dari 'Motto'?", "id": "Moto" },
    { "en": "Kata Baku Dari 'Mupakat'?", "id": "Mufakat" },
    { "en": "Kata Baku Dari 'Muqadimah'?", "id": "Mukadimah" },
    { "en": "Kata Baku Dari 'Nangkring'?", "id": "Bertengger" },
    { "en": "Kata Baku Dari 'Napaktilas'?", "id": "Napak Tilas" },
    { "en": "Kata Baku Dari 'Napi'?", "id": "Narapidana" },
    { "en": "Kata Baku Dari 'Nara Sumber'?", "id": "Narasumber" },
    { "en": "Kata Baku Dari 'Narsis'?", "id": "Narsisis" },
    { "en": "Kata Baku Dari 'Nasionalisma'?", "id": "Nasionalisme" },
    { "en": "Kata Baku Dari 'Nda'?", "id": "Tidak" },
    { "en": "Kata Baku Dari 'Ndeso'?", "id": "Kampungan" },
    { "en": "Kata Baku Dari 'Ngefans'?", "id": "Menggemari" },
    { "en": "Kata Baku Dari 'Ngeh'?", "id": "Sadar" },
    { "en": "Kata Baku Dari 'Ngekos'?", "id": "Indekos" },
    { "en": "Kata Baku Dari 'Ngelantur'?", "id": "Meresacau" },
    { "en": "Kata Baku Dari 'Ngelem'?", "id": "Mengisap Lem" },
    { "en": "Kata Baku Dari 'Ngetop'?", "id": "Terkenal" },
    { "en": "Kata Baku Dari 'Ngomong'?", "id": "Berbicara" },
    { "en": "Kata Baku Dari 'Ngontrak'?", "id": "Mengontrak" },
    { "en": "Kata Baku Dari 'Ngopi'?", "id": "Minum Kopi" },
    { "en": "Kata Baku Dari 'Ngotot'?", "id": "Bersikeras" },
    { "en": "Kata Baku Dari 'Ngrumpi'?", "id": "Bergosip" },
    { "en": "Kata Baku Dari 'Niat'?", "id": "Niat" },
    { "en": "Kata Baku Dari 'Nifas'?", "id": "Nifas" },
    { "en": "Kata Baku Dari 'Nihil'?", "id": "Nihil" },
    { "en": "Kata Baku Dari 'Nikah'?", "id": "Nikah" },
    { "en": "Kata Baku Dari 'Nilai'?", "id": "Nilai" },
    { "en": "Kata Baku Dari 'Nimbul'?", "id": "Timbul" },
    { "en": "Kata Baku Dari 'Nimbrung'?", "id": "Bergabung" },
    { "en": "Kata Baku Dari 'Nira'?", "id": "Nira" },
    { "en": "Kata Baku Dari 'Nirwana'?", "id": "Nirwana" },
    { "en": "Kata Baku Dari 'Niscaya'?", "id": "Niscaya" },
    { "en": "Kata Baku Dari 'Noda'?", "id": "Noda" },
    { "en": "Kata Baku Dari 'Nominasi'?", "id": "Nominasi" },
    { "en": "Kata Baku Dari 'Non Formal'?", "id": "Nonformal" },
    { "en": "Kata Baku Dari 'Non Stop'?", "id": "Nontsop" },
    { "en": "Kata Baku Dari 'Normalisasi'?", "id": "Normalisasi" },
    { "en": "Kata Baku Dari 'Nostalgia'?", "id": "Nostalgia" },
    { "en": "Kata Baku Dari 'Notaris'?", "id": "Notaris" },
    { "en": "Kata Baku Dari 'Note'?", "id": "Catatan" },
    { "en": "Kata Baku Dari 'Novel'?", "id": "Novel" },
    { "en": "Kata Baku Dari 'Nuansa'?", "id": "Nuansa" },
    { "en": "Kata Baku Dari 'Nuklir'?", "id": "Nuklir" },
    { "en": "Kata Baku Dari 'Numerik'?", "id": "Numerik" },
    { "en": "Kata Baku Dari 'Nusantara'?", "id": "Nusantara" },
    { "en": "Kata Baku Dari 'Nutrisi'?", "id": "Nutrisi" },
    { "en": "Kata Baku Dari 'Nyai'?", "id": "Nyai" },
    { "en": "Kata Baku Dari 'Nyala'?", "id": "Nyala" },
    { "en": "Kata Baku Dari 'Nyam'?", "id": "Lezat" },
    { "en": "Kata Baku Dari 'Nyamuk'?", "id": "Nyamuk" },
    { "en": "Kata Baku Dari 'Nyanyi'?", "id": "Nyanyi" },
    { "en": "Kata Baku Dari 'Nyaring'?", "id": "Nyaring" },
    { "en": "Kata Baku Dari 'Nyata'?", "id": "Nyata" },
    { "en": "Kata Baku Dari 'Nyawa'?", "id": "Nyawa" },
    { "en": "Kata Baku Dari 'Nyonya'?", "id": "Nyonya" },
    { "en": "Kata Baku Dari 'Nyiur'?", "id": "Nyiur" },
    { "en": "Kata Baku Dari 'Obat'?", "id": "Obat" },
    { "en": "Kata Baku Dari 'Obesitas'?", "id": "Obesitas" },
    { "en": "Kata Baku Dari 'Obituari'?", "id": "Obituarium" },
    { "en": "Kata Baku Dari 'Objektif'?", "id": "Objektif" },
    { "en": "Kata Baku Dari 'Obligasi'?", "id": "Obligasi" },
    { "en": "Kata Baku Dari 'Obral'?", "id": "Obral" },
    { "en": "Kata Baku Dari 'Obrol'?", "id": "Obrol" },
    { "en": "Kata Baku Dari 'Observasi'?", "id": "Observasi" },
    { "en": "Kata Baku Dari 'Ogah'?", "id": "Enggan" },
    { "en": "Kata Baku Dari 'Ojek'?", "id": "Ojek" },
    { "en": "Kata Baku Dari 'Oknum'?", "id": "Oknum" },
    { "en": "Kata Baku Dari 'Oksidan'?", "id": "Oksidan" },
    { "en": "Kata Baku Dari 'Oktober'?", "id": "Oktober" },
    { "en": "Kata Baku Dari 'Olah'?", "id": "Olah" },
    { "en": "Kata Baku Dari 'Oleh Oleh'?", "id": "Oleh-oleh" },
    { "en": "Kata Baku Dari 'Oles'?", "id": "Oles" },
    { "en": "Kata Baku Dari 'Olimpiade'?", "id": "Olimpiade" },
    { "en": "Kata Baku Dari 'Onde Onde'?", "id": "Onde-onde" },
    { "en": "Kata Baku Dari 'Oplosan'?", "id": "Campuran" },
    { "en": "Kata Baku Dari 'Opsionil'?", "id": "Opsional" },
    { "en": "Kata Baku Dari 'Optimis'?", "id": "Optimistis" },
    { "en": "Kata Baku Dari 'Order'?", "id": "Pesanan" },
    { "en": "Kata Baku Dari 'Orkes'?", "id": "Orkestra" },
    { "en": "Kata Baku Dari 'Otak Atik'?", "id": "Utak-atik" },
    { "en": "Kata Baku Dari 'Otomotip'?", "id": "Otomotif" },
    { "en": "Kata Baku Dari 'Over'?", "id": "Alih" },
    { "en": "Kata Baku Dari 'Pagi Pagi'?", "id": "Pagi-pagi" },
    { "en": "Kata Baku Dari 'Pak'?", "id": "Bapak" },
    { "en": "Kata Baku Dari 'Pake'?", "id": "Pakai" },
    { "en": "Kata Baku Dari 'Maksa'?", "id": "Memaksa" },
    { "en": "Kata Baku Dari 'Om'?", "id": "Paman" },
    { "en": "Kata Baku Dari 'Panca Sila'?", "id": "Pancasila" },
    { "en": "Kata Baku Dari 'Pinter'?", "id": "Pandai" },
    { "en": "Kata Baku Dari 'Manggil'?", "id": "Memanggil" },
    { "en": "Kata Baku Dari 'Pantes'?", "id": "Pantas" },
    { "en": "Kata Baku Dari 'Pantat'?", "id": "Bokong" },
    { "en": "Kata Baku Dari 'Mantau'?", "id": "Memantau" },
    { "en": "Kata Baku Dari 'Mantul'?", "id": "Memantul" },
    { "en": "Kata Baku Dari 'Papa'?", "id": "Ayah" },
    { "en": "Kata Baku Dari 'Partner'?", "id": "Mitra" },
    { "en": "Kata Baku Dari 'Masang'?", "id": "Memasang" },
    { "en": "Kata Baku Dari 'Pastiin'?", "id": "Memastikan" },
    { "en": "Kata Baku Dari 'Pause'?", "id": "Jeda" },
    { "en": "Kata Baku Dari 'Mecahin'?", "id": "Memecahkan" },
    { "en": "Kata Baku Dari 'Mecat'?", "id": "Memecat" },
    { "en": "Kata Baku Dari 'Pegel'?", "id": "Pegal" },
    { "en": "Kata Baku Dari 'Megang'?", "id": "Memegang" },
    { "en": "Kata Baku Dari 'Pekak'?", "id": "Tuli" },
    { "en": "Kata Baku Dari 'Alon-alon'?", "id": "Perlahan-lahan" },
    { "en": "Kata Baku Dari 'Meluk'?", "id": "Memeluk" },
    { "en": "Kata Baku Dari 'Finalti'?", "id": "Penalti" },
    { "en": "Kata Baku Dari 'Pencaksilat'?", "id": "Pencak Silat" },
    { "en": "Kata Baku Dari 'Mencar'?", "id": "Berpencar" },
    { "en": "Kata Baku Dari 'Mendam'?", "id": "Memendam" },
    { "en": "Kata Baku Dari 'Pendopo'?", "id": "Pendapa" },
    { "en": "Kata Baku Dari 'Penetrasi'?", "id": "Penetrasi" },
    { "en": "Kata Baku Dari 'Pengaruh'?", "id": "Pengaruh" },
    { "en": "Kata Baku Dari 'Pengkolan'?", "id": "Belokan" },
    { "en": "Kata Baku Dari 'Peniti'?", "id": "Peniti" },
    { "en": "Kata Baku Dari 'Penjara'?", "id": "Penjara" },
    { "en": "Kata Baku Dari 'Pensiun'?", "id": "Pensiun" },
    { "en": "Kata Baku Dari 'Pentas'?", "id": "Pentas" },
    { "en": "Kata Baku Dari 'Penting'?", "id": "Penting" },
    { "en": "Kata Baku Dari 'Pentil'?", "id": "Pentil" },
    { "en": "Kata Baku Dari 'Pentung'?", "id": "Pentung" },
    { "en": "Kata Baku Dari 'Penyu'?", "id": "Penyu" },
    { "en": "Kata Baku Dari 'Peot'?", "id": "Penyok" },
    { "en": "Kata Baku Dari 'Pepes'?", "id": "Pepes" },
    { "en": "Kata Baku Dari 'Perabot'?", "id": "Perabot" },
    { "en": "Kata Baku Dari 'Peraga'?", "id": "Peraga" },
    { "en": "Kata Baku Dari 'Perah'?", "id": "Perah" },
    { "en": "Kata Baku Dari 'Perahu'?", "id": "Perahu" },
    { "en": "Kata Baku Dari 'Perak'?", "id": "Perak" },
    { "en": "Kata Baku Dari 'Peran'?", "id": "Peran" },
    { "en": "Kata Baku Dari 'Perang'?", "id": "Perang" },
    { "en": "Kata Baku Dari 'Perangkap'?", "id": "Perangkap" },
    { "en": "Kata Baku Dari 'Peras'?", "id": "Peras" },
    { "en": "Kata Baku Dari 'Perban'?", "id": "Perban" },
    { "en": "Kata Baku Dari 'Percaya'?", "id": "Percaya" },
    { "en": "Kata Baku Dari 'Percik'?", "id": "Percik" },
    { "en": "Kata Baku Dari 'Perdana'?", "id": "Perdana" },
    { "en": "Kata Baku Dari 'Pergi'?", "id": "Pergi" },
    { "en": "Kata Baku Dari 'Pergok'?", "id": "Pergok" },
    { "en": "Kata Baku Dari 'Periksa'?", "id": "Periksa" },
    { "en": "Kata Baku Dari 'Perih'?", "id": "Perih" },
    { "en": "Kata Baku Dari 'Perkakas'?", "id": "Perkakas" },
    { "en": "Kata Baku Dari 'Perkara'?", "id": "Perkara" },
    { "en": "Kata Baku Dari 'Perkasa'?", "id": "Perkasa" },
    { "en": "Kata Baku Dari 'Perlente'?", "id": "Perlente" },
    { "en": "Kata Baku Dari 'Perlu'?", "id": "Perlu" },
    { "en": "Kata Baku Dari 'Permadani'?", "id": "Permadani" },
    { "en": "Kata Baku Dari 'Permak'?", "id": "Permak" },
    { "en": "Kata Baku Dari 'Permanen'?", "id": "Permanen" },
    { "en": "Kata Baku Dari 'Permen'?", "id": "Permen" },
    { "en": "Kata Baku Dari 'Permisi'?", "id": "Permisi" },
    { "en": "Kata Baku Dari 'Pernah'?", "id": "Pernah" },
    { "en": "Kata Baku Dari 'Peron'?", "id": "Peron" },
    { "en": "Kata Baku Dari 'Perosok'?", "id": "Perosok" },
    { "en": "Kata Baku Dari 'Pers'?", "id": "Pers" },
    { "en": "Kata Baku Dari 'Persen'?", "id": "Persen" },
    { "en": "Kata Baku Dari 'Perseptif'?", "id": "Perseptif" },
    { "en": "Kata Baku Dari 'Persis'?", "id": "Persis" },
    { "en": "Kata Baku Dari 'Persona'?", "id": "Persona" },
    { "en": "Kata Baku Dari 'Personel'?", "id": "Personel" },
    { "en": "Kata Baku Dari 'Perspektif'?", "id": "Perspektif" },
    { "en": "Kata Baku Dari 'Pertama'?", "id": "Pertama" },
    { "en": "Kata Baku Dari 'Partiwi'?", "id": "Pertiwi" },
    { "en": "Kata Baku Dari 'Perut'?", "id": "Perut" },
    { "en": "Kata Baku Dari 'Perwira'?", "id": "Perwira" },
    { "en": "Kata Baku Dari 'Pes'?", "id": "Pes" },
    { "en": "Kata Baku Dari 'Pesanggrahan'?", "id": "Pesanggrahan" },
    { "en": "Kata Baku Dari 'Pesantren'?", "id": "Pesantren" },
    { "en": "Kata Baku Dari 'Pesimis'?", "id": "Pesimistis" },
    { "en": "Kata Baku Dari 'Umpetin'?", "id": "Menyembunyikan" },
    { "en": "Kata Baku Dari 'Pialu'?", "id": "Demam" },
    { "en": "Kata Baku Dari 'Pijit'?", "id": "Pijat" },
    { "en": "Kata Baku Dari 'Milih'?", "id": "Memilih" },
    { "en": "Kata Baku Dari 'Minjam'?", "id": "Meminjam" },
    { "en": "Kata Baku Dari 'Piranti'?", "id": "Peranti" },
    { "en": "Kata Baku Dari 'Plapon'?", "id": "Plafon" },
    { "en": "Kata Baku Dari 'Plang'?", "id": "Papan Nama" },
    { "en": "Kata Baku Dari 'Plesir'?", "id": "Rekreasi" },
    { "en": "Kata Baku Dari 'Pontang Panting'?", "id": "Pontang-panting" },
    { "en": "Kata Baku Dari 'Populair'?", "id": "Populer" },
    { "en": "Kata Baku Dari 'Pori Pori'?", "id": "Pori-pori" },
    { "en": "Kata Baku Dari 'Pornographi'?", "id": "Pornografi" },
    { "en": "Kata Baku Dari 'Porto'?", "id": "Biaya Kirim" },
    { "en": "Kata Baku Dari 'Positip'?", "id": "Positif" },
    { "en": "Kata Baku Dari 'Posko'?", "id": "Pos Komando" },
    { "en": "Kata Baku Dari 'Motong'?", "id": "Memotong" },
    { "en": "Kata Baku Dari 'Power Window'?", "id": "Jendela Otomatis" },
    { "en": "Kata Baku Dari 'Prabawa'?", "id": "Wibawa" },
    { "en": "Kata Baku Dari 'Pra Karya'?", "id": "Prakarya" },
    { "en": "Kata Baku Dari 'Pra Kata'?", "id": "Prakata" },
    { "en": "Kata Baku Dari 'Pra Peradilan'?", "id": "Praperadilan" },
    { "en": "Kata Baku Dari 'Pri Bumi'?", "id": "Pribumi" },
    { "en": "Kata Baku Dari 'Primitip'?", "id": "Primitif" },
    { "en": "Kata Baku Dari 'Prioritet'?", "id": "Prioritas" },
    { "en": "Kata Baku Dari 'Privacy'?", "id": "Privasi" },
    { "en": "Kata Baku Dari 'Private'?", "id": "Privat" },
    { "en": "Kata Baku Dari 'Profile'?", "id": "Profil" },
    { "en": "Kata Baku Dari 'Prognosa'?", "id": "Prognosis" },
    { "en": "Kata Baku Dari 'Progresip'?", "id": "Progresif" },
    { "en": "Kata Baku Dari 'Prosedure'?", "id": "Prosedur" },
    { "en": "Kata Baku Dari 'Processor'?", "id": "Prosesor" },
    { "en": "Kata Baku Dari 'Protesa'?", "id": "Protesis" },
    { "en": "Kata Baku Dari 'Prototype'?", "id": "Prototipe" },
    { "en": "Kata Baku Dari 'Psiko Analisa'?", "id": "Psikoanalisis" },
    { "en": "Kata Baku Dari 'Public'?", "id": "Publik" },
    { "en": "Kata Baku Dari 'Puis'?", "id": "Puisi" },
    { "en": "Kata Baku Dari 'Muji'?", "id": "Memuji" },
    { "en": "Kata Baku Dari 'Mukul'?", "id": "Memukul" },
    { "en": "Kata Baku Dari 'Pungli'?", "id": "Pungutan Liar" },
    { "en": "Kata Baku Dari 'Mungut'?", "id": "Memungut" },
    { "en": "Kata Baku Dari 'Punten'?", "id": "Permisi" },
    { "en": "Kata Baku Dari 'Pura Pura'?", "id": "Pura-pura" },
    { "en": "Kata Baku Dari 'Purba Kala'?", "id": "Purbakala" },
    { "en": "Kata Baku Dari 'Puyeng'?", "id": "Pusing" },
    { "en": "Kata Baku Dari 'Puspa'?", "id": "Bunga" },
    { "en": "Kata Baku Dari 'Mutar'?", "id": "Memutar" },
    { "en": "Kata Baku Dari 'Mutusin'?", "id": "Memutuskan" },
    { "en": "Kata Baku Dari 'Quadran'?", "id": "Kuadran" },
    { "en": "Kata Baku Dari 'Rabat'?", "id": "Rabat" },
    { "en": "Kata Baku Dari 'Rabi'?", "id": "Kawin" },
    { "en": "Kata Baku Dari 'Rabu'?", "id": "Rabu" },
    { "en": "Kata Baku Dari 'Racun'?", "id": "Racun" },
    { "en": "Kata Baku Dari 'Radang'?", "id": "Radang" },
    { "en": "Kata Baku Dari 'Radar'?", "id": "Radar" },
    { "en": "Kata Baku Dari 'Radiasi'?", "id": "Radiasi" },
    { "en": "Kata Baku Dari 'Radikal'?", "id": "Radikal" },
    { "en": "Kata Baku Dari 'Radius'?", "id": "Radius" },
    { "en": "Kata Baku Dari 'Raga'?", "id": "Raga" },
    { "en": "Kata Baku Dari 'Ragi'?", "id": "Ragi" },
    { "en": "Kata Baku Dari 'Rahang'?", "id": "Rahang" },
    { "en": "Kata Baku Dari 'Rahayu'?", "id": "Selamat" },
    { "en": "Kata Baku Dari 'Rahib'?", "id": "Rahib" },
    { "en": "Kata Baku Dari 'Rahim'?", "id": "Rahim" },
    { "en": "Kata Baku Dari 'Raih'?", "id": "Raih" },
    { "en": "Kata Baku Dari 'Rais'?", "id": "Presiden" },
    { "en": "Kata Baku Dari 'Raja'?", "id": "Raja" },
    { "en": "Kata Baku Dari 'Rajin'?", "id": "Rajin" },
    { "en": "Kata Baku Dari 'Rajut'?", "id": "Rajut" },
    { "en": "Kata Baku Dari 'Rakyat'?", "id": "Rakyat" },
    { "en": "Kata Baku Dari 'Raksa'?", "id": "Raksa" },
    { "en": "Kata Baku Dari 'Raksasa'?", "id": "Raksasa" },
    { "en": "Kata Baku Dari 'Rakus'?", "id": "Rakus" },
    { "en": "Kata Baku Dari 'Ralat'?", "id": "Ralat" },
    { "en": "Kata Baku Dari 'Ram'?", "id": "Ram" },
    { "en": "Kata Baku Dari 'Ramah Tamah'?", "id": "Ramah Tamah" },
    { "en": "Kata Baku Dari 'Ramai'?", "id": "Ramai" },
    { "en": "Kata Baku Dari 'Ramal'?", "id": "Ramal" },
    { "en": "Kata Baku Dari 'Rambat'?", "id": "Rambat" },
    { "en": "Kata Baku Dari 'Rambo'?", "id": "Rambo" },
    { "en": "Kata Baku Dari 'Rampai'?", "id": "Rampai" },
    { "en": "Kata Baku Dari 'Ramping'?", "id": "Ramping" },
    { "en": "Kata Baku Dari 'Rampok'?", "id": "Rampok" },
    { "en": "Kata Baku Dari 'Rampung'?", "id": "Selesai" },
    { "en": "Kata Baku Dari 'Rancu'?", "id": "Rancu" },
    { "en": "Kata Baku Dari 'Randa'?", "id": "Janda" },
    { "en": "Kata Baku Dari 'Randai'?", "id": "Randai" },
    { "en": "Kata Baku Dari 'Rande'?", "id": "Kencan" },
    { "en": "Kata Baku Dari 'Randu'?", "id": "Randu" },
    { "en": "Kata Baku Dari 'Ranjang'?", "id": "Ranjang" },
    { "en": "Kata Baku Dari 'Ransel'?", "id": "Ransel" },
    { "en": "Kata Baku Dari 'Rantai'?", "id": "Rantai" },
    { "en": "Kata Baku Dari 'Rantau'?", "id": "Rantau" },
    { "en": "Kata Baku Dari 'Ranting'?", "id": "Ranting" },
    { "en": "Kata Baku Dari 'Ngerawat'?", "id": "Merawat" },
    { "en": "Kata Baku Dari 'Hari Raya'?", "id": "Hari Raya" },
    { "en": "Kata Baku Dari 'Realisma'?", "id": "Realisme" },
    { "en": "Kata Baku Dari 'Reverensi'?", "id": "Referensi" },
    { "en": "Kata Baku Dari 'Reguller'?", "id": "Reguler" },
    { "en": "Kata Baku Dari 'Record'?", "id": "Rekor" },
    { "en": "Kata Baku Dari 'Relatip'?", "id": "Relatif" },
    { "en": "Kata Baku Dari 'Relijius'?", "id": "Religius" },
    { "en": "Kata Baku Dari 'Ribet'?", "id": "Repot" },
    { "en": "Kata Baku Dari 'Representatip'?", "id": "Representatif" },
    { "en": "Kata Baku Dari 'Resign'?", "id": "Mengundurkan Diri" },
    { "en": "Kata Baku Dari 'Berisik'?", "id": "Ribut" },
    { "en": "Kata Baku Dari 'Rilek'?", "id": "Rileks" },
    { "en": "Kata Baku Dari 'Release'?", "id": "Rilis" },
    { "en": "Kata Baku Dari 'Enteng'?", "id": "Ringan" },
    { "en": "Kata Baku Dari 'Ringkes'?", "id": "Ringkas" },
    { "en": "Kata Baku Dari 'Rohaniawan'?", "id": "Rohaniwan" },
    { "en": "Kata Baku Dari 'Royalty'?", "id": "Royalti" },
    { "en": "Kata Baku Dari 'Rubu''?", "id": "Rubu" },
    { "en": "Kata Baku Dari 'Rumahtangga'?", "id": "Rumah Tangga" },
    { "en": "Kata Baku Dari 'Rungkad'?", "id": "Runtuh" },
    { "en": "Kata Baku Dari 'Rupa Rupa'?", "id": "Rupa-rupa" },
    { "en": "Kata Baku Dari 'Sobat'?", "id": "Sahabat" },
    { "en": "Kata Baku Dari 'Syahih'?", "id": "Sahih" },
    { "en": "Kata Baku Dari 'Nyahut'?", "id": "Menyahut" },
    { "en": "Kata Baku Dari 'Sajen'?", "id": "Sesajen" },
    { "en": "Kata Baku Dari 'Soleh'?", "id": "Saleh" },
    { "en": "Kata Baku Dari 'Salem'?", "id": "Salmon" },
    { "en": "Kata Baku Dari 'Nyambang'?", "id": "Menyambang" },
    { "en": "Kata Baku Dari 'Nyambat'?", "id": "Menyambat" },
    { "en": "Kata Baku Dari 'Nyambung'?", "id": "Menyambung" },
    { "en": "Kata Baku Dari 'Nyambut'?", "id": "Menyambut" },
    { "en": "Kata Baku Dari 'Sampe'?", "id": "Sampai" },
    { "en": "Kata Baku Dari 'Sampul'?", "id": "Sampul" },
    { "en": "Kata Baku Dari 'Sana'?", "id": "Sana" },
    { "en": "Kata Baku Dari 'Sandar'?", "id": "Sandar" },
    { "en": "Kata Baku Dari 'Sandera'?", "id": "Sandera" },
    { "en": "Kata Baku Dari 'Sandiwara'?", "id": "Sandiwara" },
    { "en": "Kata Baku Dari 'Sanding'?", "id": "Sanding" },
    { "en": "Kata Baku Dari 'Sangat'?", "id": "Sangat" },
    { "en": "Kata Baku Dari 'Sangga'?", "id": "Sangga" },
    { "en": "Kata Baku Dari 'Sanggah'?", "id": "Sanggah" },
    { "en": "Kata Baku Dari 'Sanggama'?", "id": "Senggama" },
    { "en": "Kata Baku Dari 'Sanggup'?", "id": "Sanggup" },
    { "en": "Kata Baku Dari 'Sanggurdi'?", "id": "Sanggurdi" },
    { "en": "Kata Baku Dari 'Sangka'?", "id": "Sangka" },
    { "en": "Kata Baku Dari 'Sangkal'?", "id": "Sangkal" },
    { "en": "Kata Baku Dari 'Sangkut'?", "id": "Sangkut" },
    { "en": "Kata Baku Dari 'Sangsi'?", "id": "Sanksi" },
    { "en": "Kata Baku Dari 'Sangu'?", "id": "Bekal" },
    { "en": "Kata Baku Dari 'Sapa'?", "id": "Sapa" },
    { "en": "Kata Baku Dari 'Sapi'?", "id": "Sapi" },
    { "en": "Kata Baku Dari 'Sapta'?", "id": "Sapta" },
    { "en": "Kata Baku Dari 'Sapu'?", "id": "Sapu" },
    { "en": "Kata Baku Dari 'Saraf'?", "id": "Saraf" },
    { "en": "Kata Baku Dari 'Saran'?", "id": "Saran" },
    { "en": "Kata Baku Dari 'Sarang'?", "id": "Sarang" },
    { "en": "Kata Baku Dari 'Sarasehan'?", "id": "Saresehan" },
    { "en": "Kata Baku Dari 'Sarden'?", "id": "Sarden" },
    { "en": "Kata Baku Dari 'Sariawan'?", "id": "Sariawan" },
    { "en": "Kata Baku Dari 'Saring'?", "id": "Saring" },
    { "en": "Kata Baku Dari 'Sarkasme'?", "id": "Sarkasme" },
    { "en": "Kata Baku Dari 'Sarjana'?", "id": "Sarjana" },
    { "en": "Kata Baku Dari 'Sarsaparila'?", "id": "Sarsaparila" },
    { "en": "Kata Baku Dari 'Sarung'?", "id": "Sarung" },
    { "en": "Kata Baku Dari 'Sasis'?", "id": "Sasis" },
    { "en": "Kata Baku Dari 'Sastra'?", "id": "Sastra" },
    { "en": "Kata Baku Dari 'Satelit'?", "id": "Satelit" },
    { "en": "Kata Baku Dari 'Satire'?", "id": "Satire" },
    { "en": "Kata Baku Dari 'Satu'?", "id": "Satu" },
    { "en": "Kata Baku Dari 'Satuan'?", "id": "Satuan" },
    { "en": "Kata Baku Dari 'Saudara'?", "id": "Saudara" },
    { "en": "Kata Baku Dari 'Sauna'?", "id": "Sauna" },
    { "en": "Kata Baku Dari 'Saus'?", "id": "Saus" },
    { "en": "Kata Baku Dari 'Sawa'?", "id": "Ular Sawa" },
    { "en": "Kata Baku Dari 'Sawah'?", "id": "Sawah" },
    { "en": "Kata Baku Dari 'Sawan'?", "id": "Sawan" },
    { "en": "Kata Baku Dari 'Sawer'?", "id": "Sawer" },
    { "en": "Kata Baku Dari 'Sawi'?", "id": "Sawi" },
    { "en": "Kata Baku Dari 'Sawo'?", "id": "Sawo" },
    { "en": "Kata Baku Dari 'Saya'?", "id": "Saya" },
    { "en": "Kata Baku Dari 'Sayang'?", "id": "Sayang" },
    { "en": "Kata Baku Dari 'Sayap'?", "id": "Sayap" },
    { "en": "Kata Baku Dari 'Sayat'?", "id": "Sayat" },
    { "en": "Kata Baku Dari 'Sayembara'?", "id": "Sayembara" },
    { "en": "Kata Baku Dari 'Sebab'?", "id": "Sebab" },
    { "en": "Kata Baku Dari 'Sebagai'?", "id": "Sebagai" },
    { "en": "Kata Baku Dari 'Sebal'?", "id": "Sebal" },
    { "en": "Kata Baku Dari 'Sebar'?", "id": "Sebar" },
    { "en": "Kata Baku Dari 'Sebat'?", "id": "Sebat" },
    { "en": "Kata Baku Dari 'Sebelah'?", "id": "Sebelah" },
    { "en": "Kata Baku Dari 'Sebelum'?", "id": "Sebelum" },
    { "en": "Kata Baku Dari 'Nyebut'?", "id": "Menyebut" },
    { "en": "Kata Baku Dari 'Sodakoh'?", "id": "Sedekah" },
    { "en": "Kata Baku Dari 'Dikit'?", "id": "Sedikit" },
    { "en": "Kata Baku Dari 'Nyedot'?", "id": "Menyedot" },
    { "en": "Kata Baku Dari 'Segi Tiga'?", "id": "Segitiga" },
    { "en": "Kata Baku Dari 'Sekali Gus'?", "id": "Sekaligus" },
    { "en": "Kata Baku Dari 'Sekali Pun'?", "id": "Sekalipun" },
    { "en": "Kata Baku Dari 'Sekonyong Konyong'?", "id": "Sekonyong-konyong" },
    { "en": "Kata Baku Dari 'Sekular'?", "id": "Sekuler" },
    { "en": "Kata Baku Dari 'Selebriti'?", "id": "Selebritas" },
    { "en": "Kata Baku Dari 'Seletuk'?", "id": "Celetuk" },
    { "en": "Kata Baku Dari 'Nyelidik'?", "id": "Menyelidiki" },
    { "en": "Kata Baku Dari 'Selo'?", "id": "Celo" },
    { "en": "Kata Baku Dari 'Selot'?", "id": "Gerendel" },
    { "en": "Kata Baku Dari 'Seluk Beluk'?", "id": "Seluk-beluk" },
    { "en": "Kata Baku Dari 'Sembako'?", "id": "Sembilan Bahan Pokok" },
    { "en": "Kata Baku Dari 'Ngumpet'?", "id": "Bersembunyi" },
    { "en": "Kata Baku Dari 'Nyembur'?", "id": "Menyembur" },
    { "en": "Kata Baku Dari 'Smèster'?", "id": "Semester" },
    { "en": "Kata Baku Dari 'Semi Final'?", "id": "Semifinal" },
    { "en": "Kata Baku Dari 'Sempak'?", "id": "Celana Dalam" },
    { "en": "Kata Baku Dari 'Nyenggol'?", "id": "Menyenggol" },
    { "en": "Kata Baku Dari 'Senen'?", "id": "Senin" },
    { "en": "Kata Baku Dari 'Nyentak'?", "id": "Menyentak" },
    { "en": "Kata Baku Dari 'Nyentuh'?", "id": "Menyentuh" },
    { "en": "Kata Baku Dari 'Nyepak'?", "id": "Menyepak" },
    { "en": "Kata Baku Dari 'Separo'?", "id": "Separuh" },
    { "en": "Kata Baku Dari 'Kayak'?", "id": "Seperti" },
    { "en": "Kata Baku Dari 'Seper Empat'?", "id": "Seperempat" },
    { "en": "Kata Baku Dari 'Nyerah'?", "id": "Menyerah" },
    { "en": "Kata Baku Dari 'Nyerang'?", "id": "Menyerang" },
    { "en": "Kata Baku Dari 'Nyerbu'?", "id": "Menyerbu" },
    { "en": "Kata Baku Dari 'Nyeret'?", "id": "Menyeret" },
    { "en": "Kata Baku Dari 'Serba Salah'?", "id": "Serbasalah" },
    { "en": "Kata Baku Dari 'Serba Serbi'?", "id": "Serbaserbi" },
    { "en": "Kata Baku Dari 'Sereal'?", "id": "Serealia" },
    { "en": "Kata Baku Dari 'Seremoni'?", "id": "Seremoni" },
    { "en": "Kata Baku Dari 'Nyergap'?", "id": "Menyergap" },
    { "en": "Kata Baku Dari 'Sarekat'?", "id": "Serikat" },
    { "en": "Kata Baku Dari 'Seringkali'?", "id": "Sering Kali" },
    { "en": "Kata Baku Dari 'Nyerobot'?", "id": "Menyerobot" },
    { "en": "Kata Baku Dari 'Sertu'?", "id": "Sersan Satu" },
    { "en": "Kata Baku Dari 'Serper'?", "id": "Server" },
    { "en": "Kata Baku Dari 'Sesek'?", "id": "Sesak" },
    { "en": "Kata Baku Dari 'Nyesal'?", "id": "Menyesal" },
    { "en": "Kata Baku Dari 'Sesepuh'?", "id": "Pinisepuh" },
    { "en": "Kata Baku Dari 'Abis'?", "id": "Setelah" },
    { "en": "Kata Baku Dari 'Setan'?", "id": "Setan" },
    { "en": "Kata Baku Dari 'Setaraf'?", "id": "Setaraf" },
    { "en": "Kata Baku Dari 'Seteguk'?", "id": "Seteguk" },
    { "en": "Kata Baku Dari 'Setengah'?", "id": "Setengah" },
    { "en": "Kata Baku Dari 'Setentang'?", "id": "Setentang" },
    { "en": "Kata Baku Dari 'Setia'?", "id": "Setia" },
    { "en": "Kata Baku Dari 'Setiap'?", "id": "Setiap" },
    { "en": "Kata Baku Dari 'Setir'?", "id": "Setir" },
    { "en": "Kata Baku Dari 'Setop'?", "id": "Setop" },
    { "en": "Kata Baku Dari 'Setrika'?", "id": "Setrika" },
    { "en": "Kata Baku Dari 'Setrum'?", "id": "Setrum" },
    { "en": "Kata Baku Dari 'Setuju'?", "id": "Setuju" },
    { "en": "Kata Baku Dari 'Sewa'?", "id": "Sewa" },
    { "en": "Kata Baku Dari 'Sewot'?", "id": "Marah" },
    { "en": "Kata Baku Dari 'Siaga'?", "id": "Siaga" },
    { "en": "Kata Baku Dari 'Siang'?", "id": "Siang" },
    { "en": "Kata Baku Dari 'Sianida'?", "id": "Sianida" },
    { "en": "Kata Baku Dari 'Siap'?", "id": "Siap" },
    { "en": "Kata Baku Dari 'Siar'?", "id": "Siar" },
    { "en": "Kata Baku Dari 'Siasat'?", "id": "Siasat" },
    { "en": "Kata Baku Dari 'Sibir'?", "id": "Sibir" },
    { "en": "Kata Baku Dari 'Sibuk'?", "id": "Sibuk" },
    { "en": "Kata Baku Dari 'Sidak'?", "id": "Inspeksi Mendadak" },
    { "en": "Kata Baku Dari 'Sidang'?", "id": "Sidang" },
    { "en": "Kata Baku Dari 'Sifat'?", "id": "Sifat" },
    { "en": "Kata Baku Dari 'Sihir'?", "id": "Sihir" },
    { "en": "Kata Baku Dari 'Sikap'?", "id": "Sikap" },
    { "en": "Kata Baku Dari 'Sikat'?", "id": "Sikat" },
    { "en": "Kata Baku Dari 'Siksa'?", "id": "Siksa" },
    { "en": "Kata Baku Dari 'Siku'?", "id": "Siku" },
    { "en": "Kata Baku Dari 'Sila'?", "id": "Sila" },
    { "en": "Kata Baku Dari 'Silabus'?", "id": "Silabus" },
    { "en": "Kata Baku Dari 'Silam'?", "id": "Silam" },
    { "en": "Kata Baku Dari 'Silang'?", "id": "Silang" },
    { "en": "Kata Baku Dari 'Silat'?", "id": "Silat" },
    { "en": "Kata Baku Dari 'Silau'?", "id": "Silau" },
    { "en": "Kata Baku Dari 'Silih'?", "id": "Silih" },
    { "en": "Kata Baku Dari 'Silinder'?", "id": "Silinder" },
    { "en": "Kata Baku Dari 'Simak'?", "id": "Simak" },
    { "en": "Kata Baku Dari 'Simbah'?", "id": "Kakek" },
    { "en": "Kata Baku Dari 'Simpan'?", "id": "Simpan" },
    { "en": "Kata Baku Dari 'Simpang'?", "id": "Simpang" },
    { "en": "Kata Baku Dari 'Simpanse'?", "id": "Simpanse" },
    { "en": "Kata Baku Dari 'Simpati'?", "id": "Simpati" },
    { "en": "Kata Baku Dari 'Simpir'?", "id": "Simpir" },
    { "en": "Kata Baku Dari 'Simpul'?", "id": "Simpul" },
    { "en": "Kata Baku Dari 'Simsalabim'?", "id": "Simsalabim" },
    { "en": "Kata Baku Dari 'Simulasi'?", "id": "Simulasi" },
    { "en": "Kata Baku Dari 'Nyindir'?", "id": "Menyindir" },
    { "en": "Kata Baku Dari 'Nyinggung'?", "id": "Menyinggung" },
    { "en": "Kata Baku Dari 'Nyingkir'?", "id": "Menyingkir" },
    { "en": "Kata Baku Dari 'Kesini'?", "id": "Ke Sini" },
    { "en": "Kata Baku Dari 'Sinyo'?", "id": "Tuan Muda" },
    { "en": "Kata Baku Dari 'Sipoa'?", "id": "Swipoa" },
    { "en": "Kata Baku Dari 'Nyiram'?", "id": "Menyiram" },
    { "en": "Kata Baku Dari 'Nyisir'?", "id": "Menyisir" },
    { "en": "Kata Baku Dari 'Nyita'?", "id": "Menyita" },
    { "en": "Kata Baku Dari 'Kesitu'?", "id": "Ke Situ" },
    { "en": "Kata Baku Dari 'Siwer'?", "id": "Silau" },
    { "en": "Kata Baku Dari 'Slengean'?", "id": "Urakan" },
    { "en": "Kata Baku Dari 'Nyodorin'?", "id": "Menyodorkan" },
    { "en": "Kata Baku Dari 'Nyogok'?", "id": "Menyogok" },
    { "en": "Kata Baku Dari 'Nyokong'?", "id": "Menyokong" },
    { "en": "Kata Baku Dari 'Solideritas'?", "id": "Solidaritas" },
    { "en": "Kata Baku Dari 'Sop'?", "id": "Sup" },
    { "en": "Kata Baku Dari 'Sorak Sorai'?", "id": "Sorak-sorai" },
    { "en": "Kata Baku Dari 'Nyorot'?", "id": "Menyorot" },
    { "en": "Kata Baku Dari 'Soun'?", "id": "Sohun" },
    { "en": "Kata Baku Dari 'Stocking'?", "id": "Stoking" },
    { "en": "Kata Baku Dari 'Stadium'?", "id": "Stadion" },
    { "en": "Kata Baku Dari 'Stand'?", "id": "Stan" },
    { "en": "Kata Baku Dari 'Start'?", "id": "Mulai" },
    { "en": "Kata Baku Dari 'Steak'?", "id": "Bistik" },
    { "en": "Kata Baku Dari 'Stop'?", "id": "Berhenti" },
    { "en": "Kata Baku Dari 'Stop Kontak'?", "id": "Stopkontak" },
    { "en": "Kata Baku Dari 'Nyuap'?", "id": "Menyuap" },
    { "en": "Kata Baku Dari 'Sub Kontraktor'?", "id": "Subkontraktor" },
    { "en": "Kata Baku Dari 'Sub Seksi'?", "id": "Subseksi" },
    { "en": "Kata Baku Dari 'Udah'?", "id": "Sudah" },
    { "en": "Kata Baku Dari 'Demen'?", "id": "Suka" },
    { "en": "Kata Baku Dari 'Nyulam'?", "id": "Menyulam" },
    { "en": "Kata Baku Dari 'Susah'?", "id": "Sulit" },
    { "en": "Kata Baku Dari 'Sulfur'?", "id": "Belerang" },
    { "en": "Kata Baku Dari 'Nyumpal'?", "id": "Menyumpal" },
    { "en": "Kata Baku Dari 'Sumpek'?", "id": "Sesak" },
    { "en": "Kata Baku Dari 'Beneran'?", "id": "Sungguh" },
    { "en": "Kata Baku Dari 'Sungkan'?", "id": "Segan" },
    { "en": "Kata Baku Dari 'Super Market'?", "id": "Supermarket" },
    { "en": "Kata Baku Dari 'Super Power'?", "id": "Adi Kuasa" },
    { "en": "Kata Baku Dari 'Surfing'?", "id": "Berselancar" },
    { "en": "Kata Baku Dari 'Nyuruh'?", "id": "Menyuruh" },
    { "en": "Kata Baku Dari 'Nyusul'?", "id": "Menyusul" },
    { "en": "Kata Baku Dari 'Nyusun'?", "id": "Menyusun" },
    { "en": "Kata Baku Dari 'Nyusup'?", "id": "Menyusup" },
    { "en": "Kata Baku Dari 'Souvenir'?", "id": "Suvenir" },
    { "en": "Kata Baku Dari 'Selfie'?", "id": "Swafoto" },
    { "en": "Kata Baku Dari 'Swa Kelola'?", "id": "Swakelola" },
    { "en": "Kata Baku Dari 'Swa Sembada'?", "id": "Swasembada" },
    { "en": "Kata Baku Dari 'Sahdu'?", "id": "Syahdu" },
    { "en": "Kata Baku Dari 'Syi'ar'?", "id": "Siar" },
    { "en": "Kata Baku Dari 'Syur'?", "id": "Seksi" },
    { "en": "Kata Baku Dari 'Tabok'?", "id": "Tampar" },
    { "en": "Kata Baku Dari 'Tadi'?", "id": "Tadi" },
    { "en": "Kata Baku Dari 'Tafakur'?", "id": "Tafakur" },
    { "en": "Kata Baku Dari 'Tagih'?", "id": "Tagih" },
    { "en": "Kata Baku Dari 'Tahan'?", "id": "Tahan" },
    { "en": "Kata Baku Dari 'Tahayul'?", "id": "Takhayul" },
    { "en": "Kata Baku Dari 'Tahlil'?", "id": "Tahlil" },
    { "en": "Kata Baku Dari 'Tahu'?", "id": "Tahu" },
    { "en": "Kata Baku Dari 'Tahun'?", "id": "Tahun" },
    { "en": "Kata Baku Dari 'Taifun'?", "id": "Taufan" },
    { "en": "Kata Baku Dari 'Taikong'?", "id": "Taikong" },
    { "en": "Kata Baku Dari 'Ta'jil'?", "id": "Takjil" },
    { "en": "Kata Baku Dari 'Takar'?", "id": "Takar" },
    { "en": "Kata Baku Dari 'Takbir'?", "id": "Takbir" },
    { "en": "Kata Baku Dari 'Takhta'?", "id": "Takhta" },
    { "en": "Kata Baku Dari 'Takjub'?", "id": "Takjub" },
    { "en": "Kata Baku Dari 'Taklid'?", "id": "Taklid" },
    { "en": "Kata Baku Dari 'Takluk'?", "id": "Takluk" },
    { "en": "Kata Baku Dari 'Takraw'?", "id": "Takraw" },
    { "en": "Kata Baku Dari 'Taksi'?", "id": "Taksi" },
    { "en": "Kata Baku Dari 'Taksir'?", "id": "Taksir" },
    { "en": "Kata Baku Dari 'Taktik'?", "id": "Taktik" },
    { "en": "Kata Baku Dari 'Takut'?", "id": "Takut" },
    { "en": "Kata Baku Dari 'Takwa'?", "id": "Takwa" },
    { "en": "Kata Baku Dari 'Takzim'?", "id": "Takzim" },
    { "en": "Kata Baku Dari 'Talas'?", "id": "Talas" },
    { "en": "Kata Baku Dari 'Talak'?", "id": "Talak" },
    { "en": "Kata Baku Dari 'Talam'?", "id": "Talam" },
    { "en": "Kata Baku Dari 'Talenta'?", "id": "Talenta" },
    { "en": "Kata Baku Dari 'Tali'?", "id": "Tali" },
    { "en": "Kata Baku Dari 'Tam'?", "id": "Stempel" },
    { "en": "Kata Baku Dari 'Taman'?", "id": "Taman" },
    { "en": "Kata Baku Dari 'Tamat'?", "id": "Tamat" },
    { "en": "Kata Baku Dari 'Tambah'?", "id": "Tambah" },
    { "en": "Kata Baku Dari 'Tambak'?", "id": "Tambak" },
    { "en": "Kata Baku Dari 'Tambal'?", "id": "Tambal" },
    { "en": "Kata Baku Dari 'Tambang'?", "id": "Tambang" },
    { "en": "Kata Baku Dari 'Tambat'?", "id": "Tambat" },
    { "en": "Kata Baku Dari 'Tambo'?", "id": "Tambo" },
    { "en": "Kata Baku Dari 'Tamborin'?", "id": "Tamborin" },
    { "en": "Kata Baku Dari 'Tambun'?", "id": "Tambun" },
    { "en": "Kata Baku Dari 'Tampak'?", "id": "Tampak" },
    { "en": "Kata Baku Dari 'Tampan'?", "id": "Tampan" },
    { "en": "Kata Baku Dari 'Tampar'?", "id": "Tampar" },
    { "en": "Kata Baku Dari 'Nampung'?", "id": "Menampung" },
    { "en": "Kata Baku Dari 'Nanam'?", "id": "Menanam" },
    { "en": "Kata Baku Dari 'Tanahair'?", "id": "Tanah Air" },
    { "en": "Kata Baku Dari 'Nancap'?", "id": "Menancap" },
    { "en": "Kata Baku Dari 'Tanda Tangan'?", "id": "Tanda Tangan" },
    { "en": "Kata Baku Dari 'Nandingin'?", "id": "Menandingi" },
    { "en": "Kata Baku Dari 'Nanduk'?", "id": "Menanduk" },
    { "en": "Kata Baku Dari 'Nanggung'?", "id": "Menanggung" },
    { "en": "Kata Baku Dari 'Nangis'?", "id": "Menangis" },
    { "en": "Kata Baku Dari 'Nangkal'?", "id": "Menangkal" },
    { "en": "Kata Baku Dari 'Nangkap'?", "id": "Menangkap" },
    { "en": "Kata Baku Dari 'Nangkis'?", "id": "Menangkis" },
    { "en": "Kata Baku Dari 'Tante'?", "id": "Bibi" },
    { "en": "Kata Baku Dari 'Nantang'?", "id": "Menantang" },
    { "en": "Kata Baku Dari 'Nenun'?", "id": "Menenun" },
    { "en": "Kata Baku Dari 'Tepuktangan'?", "id": "Tepuk Tangan" },
    { "en": "Kata Baku Dari 'Nerka'?", "id": "Menerka" },
    { "en": "Kata Baku Dari 'Nerapin'?", "id": "Menerapkan" },
    { "en": "Kata Baku Dari 'Nerima'?", "id": "Menerima" },
    { "en": "Kata Baku Dari 'Nerkam'?", "id": "Menerkam" },
    { "en": "Kata Baku Dari 'Terkadang'?", "id": "Terkadang" },
    { "en": "Kata Baku Dari 'Neropong'?", "id": "Meneropong" },
    { "en": "Kata Baku Dari 'Nerpa'?", "id": "Menerpa" },
    { "en": "Kata Baku Dari 'Terus-terusan'?", "id": "Terus-menerus" },
    { "en": "Kata Baku Dari 'Ngetes'?", "id": "Menguji" },
    { "en": "Kata Baku Dari 'Netes'?", "id": "Menetes" },
    { "en": "Kata Baku Dari 'Nyampe'?", "id": "Tiba" },
    { "en": "Kata Baku Dari 'Bobo'?", "id": "Tidur" },
    { "en": "Kata Baku Dari 'Tipes'?", "id": "Tifus" },
    { "en": "Kata Baku Dari 'Nikam'?", "id": "Menikam" },
    { "en": "Kata Baku Dari 'Nikung'?", "id": "Menikung" },
    { "en": "Kata Baku Dari 'Nilap'?", "id": "Menilap" },
    { "en": "Kata Baku Dari 'Nimba'?", "id": "Menimba" },
    { "en": "Kata Baku Dari 'Nimbang'?", "id": "Menimbang" },
    { "en": "Kata Baku Dari 'Nimpa'?", "id": "Menimpa" },
    { "en": "Kata Baku Dari 'Nindak'?", "id": "Menindak" },
    { "en": "Kata Baku Dari 'Nindas'?", "id": "Menindas" },
    { "en": "Kata Baku Dari 'Nindih'?", "id": "Menindih" },
    { "en": "Kata Baku Dari 'Ninggalin'?", "id": "Meninggalkan" },
    { "en": "Kata Baku Dari 'Ninjau'?", "id": "Meninjau" },
    { "en": "Kata Baku Dari 'Nonjok'?", "id": "Meninju" },
    { "en": "Kata Baku Dari 'Niru'?", "id": "Meniru" },
    { "en": "Kata Baku Dari 'Tissue'?", "id": "Tisu" },
    { "en": "Kata Baku Dari 'Titik Dua'?", "id": "Titik Dua" },
    { "en": "Kata Baku Dari 'Titikberat'?", "id": "Titik Berat" },
    { "en": "Kata Baku Dari 'Nitip'?", "id": "Menitip" },
    { "en": "Kata Baku Dari 'Nitis'?", "id": "Menitis" },
    { "en": "Kata Baku Dari 'Niup'?", "id": "Meniup" },
    { "en": "Kata Baku Dari 'Tokcer'?", "id": "Manjur" },
    { "en": "Kata Baku Dari 'Nodong'?", "id": "Menodong" },
    { "en": "Kata Baku Dari 'Nolak'?", "id": "Menolak" },
    { "en": "Kata Baku Dari 'Tolakpeluru'?", "id": "Tolak Peluru" },
    { "en": "Kata Baku Dari 'Noleh'?", "id": "Menoleh" },
    { "en": "Kata Baku Dari 'Nolong'?", "id": "Menolong" },
    { "en": "Kata Baku Dari 'Nonjol'?", "id": "Menonjol" },
    { "en": "Kata Baku Dari 'Nonton'?", "id": "Menonton" },
    { "en": "Kata Baku Dari 'Nopang'?", "id": "Menopang" },
    { "en": "Kata Baku Dari 'Noreh'?", "id": "Menoreh" },
    { "en": "Kata Baku Dari 'Totaliterisme'?", "id": "Totalitarianisme" },
    { "en": "Kata Baku Dari 'Trakteer'?", "id": "Traktir" },
    { "en": "Kata Baku Dari 'Mentransfer'?", "id": "Menransfer" },
    { "en": "Kata Baku Dari 'Tuberkulosa'?", "id": "Tuberkulosis" },
    { "en": "Kata Baku Dari 'Nubruk'?", "id": "Menubruk" },
    { "en": "Kata Baku Dari 'Nuduh'?", "id": "Menuduh" },
    { "en": "Kata Baku Dari 'Nuding'?", "id": "Menuding" },
    { "en": "Kata Baku Dari 'Nuju'?", "id": "Menuju" },
    { "en": "Kata Baku Dari 'Nukar'?", "id": "Menukar" },
    { "en": "Kata Baku Dari 'Tukarguling'?", "id": "Tukar Guling" },
    { "en": "Kata Baku Dari 'Nular'?", "id": "Menular" },
    { "en": "Kata Baku Dari 'Nulis'?", "id": "Menulis" },
    { "en": "Kata Baku Dari 'Tumbang'?", "id": "Tumbang" },
    { "en": "Kata Baku Dari 'Tumben'?", "id": "Tumben" },
    { "en": "Kata Baku Dari 'Tumbuh'?", "id": "Tumbuh" },
    { "en": "Kata Baku Dari 'Tumbuk'?", "id": "Tumbuk" },
    { "en": "Kata Baku Dari 'Tumis'?", "id": "Tumis" },
    { "en": "Kata Baku Dari 'Tumit'?", "id": "Tumit" },
    { "en": "Kata Baku Dari 'Tumor'?", "id": "Tumor" },
    { "en": "Kata Baku Dari 'Tumpah'?", "id": "Tumpah" },
    { "en": "Kata Baku Dari 'Tumpang'?", "id": "Tumpang" },
    { "en": "Kata Baku Dari 'Tumpas'?", "id": "Tumpas" },
    { "en": "Kata Baku Dari 'Tumpul'?", "id": "Tumpul" },
    { "en": "Kata Baku Dari 'Tumpuk'?", "id": "Tumpuk" },
    { "en": "Kata Baku Dari 'Tunai'?", "id": "Tunai" },
    { "en": "Kata Baku Dari 'Tunanetra'?", "id": "Tunanetra" },
    { "en": "Kata Baku Dari 'Tunas'?", "id": "Tunas" },
    { "en": "Kata Baku Dari 'Tunda'?", "id": "Tunda" },
    { "en": "Kata Baku Dari 'Tundra'?", "id": "Tundra" },
    { "en": "Kata Baku Dari 'Tungau'?", "id": "Tungau" },
    { "en": "Kata Baku Dari 'Tunggal'?", "id": "Tunggal" },
    { "en": "Kata Baku Dari 'Tunggang'?", "id": "Tunggang" },
    { "en": "Kata Baku Dari 'Tunggu'?", "id": "Tunggu" },
    { "en": "Kata Baku Dari 'Tungkai'?", "id": "Tungkai" },
    { "en": "Kata Baku Dari 'Tungku'?", "id": "Tungku" },
    { "en": "Kata Baku Dari 'Tunjuk'?", "id": "Tunjuk" },
    { "en": "Kata Baku Dari 'Tuntas'?", "id": "Tuntas" },
    { "en": "Kata Baku Dari 'Tuntut'?", "id": "Tuntut" },
    { "en": "Kata Baku Dari 'Tupai'?", "id": "Tupai" },
    { "en": "Kata Baku Dari 'Nurunin'?", "id": "Menurunkan" },
    { "en": "Kata Baku Dari 'Turun Temurun'?", "id": "Turun-temurun" },
    { "en": "Kata Baku Dari 'Nusuk'?", "id": "Menusuk" },
    { "en": "Kata Baku Dari 'Tuter'?", "id": "Klakson" },
    { "en": "Kata Baku Dari 'Ubun Ubun'?", "id": "Ubun-ubun" },
    { "en": "Kata Baku Dari 'Ngucapin'?", "id": "Mengucapkan" },
    { "en": "Kata Baku Dari 'Ngukir'?", "id": "Mengukir" },
    { "en": "Kata Baku Dari 'Ngukur'?", "id": "Mengukur" },
    { "en": "Kata Baku Dari 'Ngulang'?", "id": "Mengulang" },
    { "en": "Kata Baku Dari 'Ulang Alik'?", "id": "Ulang-alik" },
    { "en": "Kata Baku Dari 'Ngulas'?", "id": "Mengulas" },
    { "en": "Kata Baku Dari 'Ulem'?", "id": "Undangan" },
    { "en": "Kata Baku Dari 'Ngumpat'?", "id": "Mengumpat" },
    { "en": "Kata Baku Dari 'Ngunjuk'?", "id": "Mengunjuk" },
    { "en": "Kata Baku Dari 'Upacar Bendera'?", "id": "Upacara Bendera" },
    { "en": "Kata Baku Dari 'Upload'?", "id": "Unggah" },
    { "en": "Kata Baku Dari 'Ngurai'?", "id": "Mengurai" },
    { "en": "Kata Baku Dari 'Urgen'?", "id": "Mendesak" },
    { "en": "Kata Baku Dari 'Ngurus'?", "id": "Mengurus" },
    { "en": "Kata Baku Dari 'Ngurut'?", "id": "Mengurut" },
    { "en": "Kata Baku Dari 'Ngusap'?", "id": "Mengusap" },
    { "en": "Kata Baku Dari 'Ngusik'?", "id": "Mengusik" },
    { "en": "Kata Baku Dari 'Ngusir'?", "id": "Mengusir" },
    { "en": "Kata Baku Dari 'Ngusulin'?", "id": "Mengusulkan" },
    { "en": "Kata Baku Dari 'Ngusung'?", "id": "Mengusung" },
    { "en": "Kata Baku Dari 'Ngusut'?", "id": "Mengusut" },
    { "en": "Kata Baku Dari 'Faksinasi'?", "id": "Vaksinasi" },
    { "en": "Kata Baku Dari 'Valas'?", "id": "Valuta Asing" },
    { "en": "Kata Baku Dari 'Palid'?", "id": "Valid" },
    { "en": "Kata Baku Dari 'Pampir'?", "id": "Vampir" },
    { "en": "Kata Baku Dari 'Pandalisme'?", "id": "Vandalisme" },
    { "en": "Kata Baku Dari 'Paselin'?", "id": "Vaselin" },
    { "en": "Kata Baku Dari 'Pentilator'?", "id": "Ventilator" },
    { "en": "Kata Baku Dari 'Vihara'?", "id": "Wihara" },
    { "en": "Kata Baku Dari 'Volunter'?", "id": "Sukarelawan" },
    { "en": "Kata Baku Dari 'Voting'?", "id": "Pemungutan Suara" },
    { "en": "Kata Baku Dari 'Vulkanisir'?", "id": "Vulkanisasi" },
    { "en": "Kata Baku Dari 'Wagon'?", "id": "Gerbong" },
    { "en": "Kata Baku Dari 'Wara Laba'?", "id": "Waralaba" },
    { "en": "Kata Baku Dari 'Warga Negara'?", "id": "Warganegara" },
    { "en": "Kata Baku Dari 'Waringin'?", "id": "Beringin" },
    { "en": "Kata Baku Dari 'Warteg'?", "id": "Warung Tegal" },
    { "en": "Kata Baku Dari 'Wartel'?", "id": "Warung Telekomunikasi" },
    { "en": "Kata Baku Dari 'Webcam'?", "id": "Kamera Web" },
    { "en": "Kata Baku Dari 'Website'?", "id": "Situs Web" },
    { "en": "Kata Baku Dari 'Weekend'?", "id": "Akhir Pekan" },
    { "en": "Kata Baku Dari 'Weker'?", "id": "Jam Beker" },
    { "en": "Kata Baku Dari 'Welder'?", "id": "Juru Las" },
    { "en": "Kata Baku Dari 'Widyawisata'?", "id": "Studi Tur" },
    { "en": "Kata Baku Dari 'Wifi'?", "id": "Wi-fi" },
    { "en": "Kata Baku Dari 'Senofobia'?", "id": "Xenofobia" },
    { "en": "Kata Baku Dari 'Yacht'?", "id": "Kapal Pesiar" },
    { "en": "Kata Baku Dari 'Yahud'?", "id": "Hebat" },
    { "en": "Kata Baku Dari 'Yg'?", "id": "Yang" },
    { "en": "Kata Baku Dari 'Yuris'?", "id": "Ahli Hukum" },
    { "en": "Kata Baku Dari 'Anastesi'?", "id": "Anestesi" },
    { "en": "Kata Baku Dari 'Caddy'?", "id": "Pramugolf" },
    { "en": "Kata Baku Dari 'Dashboard'?", "id": "Dasbor" },
    { "en": "Kata Baku Dari 'De Facto'?", "id": "De Facto" },
    { "en": "Kata Baku Dari 'De Jure'?", "id": "De Jure" },
    { "en": "Kata Baku Dari 'Error'?", "id": "Galat" },
    { "en": "Kata Baku Dari 'Fashion'?", "id": "Fesyen" },
    { "en": "Kata Baku Dari 'Gadget'?", "id": "Gawai" },
    { "en": "Kata Baku Dari 'Geotermal'?", "id": "Panas Bumi" },
    { "en": "Kata Baku Dari 'Hattrick'?", "id": "Trigol" },
    { "en": "Kata Baku Dari 'Headphone'?", "id": "Pelantang Telinga" },
    { "en": "Kata Baku Dari 'Headline'?", "id": "Tajuk Rencana" },
    { "en": "Kata Baku Dari 'Hellycopter'?", "id": "Helikopter" },
    { "en": "Kata Baku Dari 'Herball'?", "id": "Herbal" },
    { "en": "Kata Baku Dari 'Hetrogen'?", "id": "Heterogen" },
    { "en": "Kata Baku Dari 'Hirarki'?", "id": "Hierarki" },
    { "en": "Kata Baku Dari 'Hiper Aktif'?", "id": "Hiperaktif" },
    { "en": "Kata Baku Dari 'Hiper Tensi'?", "id": "Hipertensi" },
    { "en": "Kata Baku Dari 'Hipokrit'?", "id": "Munafik" },
    { "en": "Kata Baku Dari 'Homepage'?", "id": "Laman" },
    { "en": "Kata Baku Dari 'Hoax'?", "id": "Hoaks" },
    { "en": "Kata Baku Dari 'Horisontal'?", "id": "Horizontal" },
    { "en": "Kata Baku Dari 'Hostes'?", "id": "Pramuria" },
    { "en": "Kata Baku Dari 'Hotspot'?", "id": "Hotspot" },
    { "en": "Kata Baku Dari 'Idola'?", "id": "Idola" },
    { "en": "Kata Baku Dari 'Image'?", "id": "Citra" },
    { "en": "Kata Baku Dari 'Impeachment'?", "id": "Pemakzulan" },
    { "en": "Kata Baku Dari 'In Put'?", "id": "Masukan" },
    { "en": "Kata Baku Dari 'Incognito'?", "id": "Penyamaran" },
    { "en": "Kata Baku Dari 'Infotaiment'?", "id": "Informatika Hiburan" },
    { "en": "Kata Baku Dari 'Infrastruktur'?", "id": "Infrastruktur" },
    { "en": "Kata Baku Dari 'Inisial'?", "id": "Inisial" },
    { "en": "Kata Baku Dari 'Inline'?", "id": "Daring" },
    { "en": "Kata Baku Dari 'Input'?", "id": "Masukan" },
    { "en": "Kata Baku Dari 'Instal'?", "id": "Instal" },
    { "en": "Kata Baku Dari 'Insyaallah'?", "id": "Insyaallah" },
    { "en": "Kata Baku Dari 'Interbis'?", "id": "Antarbus" },
    { "en": "Kata Baku Dari 'Intermeso'?", "id": "Intermeso" },
    { "en": "Kata Baku Dari 'Internet'?", "id": "Internet" },
    { "en": "Kata Baku Dari 'Interogasi'?", "id": "Interogasi" },
    { "en": "Kata Baku Dari 'Interupsi'?", "id": "Interupsi" },
    { "en": "Kata Baku Dari 'Inti'?", "id": "Inti" },
    { "en": "Kata Baku Dari 'Akhli'?", "id": "Ahli" },
    { "en": "Kata Baku Dari 'Ambeyen'?", "id": "Ambeien" },
    { "en": "Kata Baku Dari 'Branghang'?", "id": "Berangasan" },
    { "en": "Kata Baku Dari 'Contek'?", "id": "Sontek" },
    { "en": "Kata Baku Dari 'Dari Pada'?", "id": "Daripada" },
    { "en": "Kata Baku Dari 'Extra'?", "id": "Ekstra" },
    { "en": "Kata Baku Dari 'Gono Gini'?", "id": "Gana-gini" },
    { "en": "Kata Baku Dari 'Griya'?", "id": "Gria" },
    { "en": "Kata Baku Dari 'Loka Karya'?", "id": "Lokakarya" },
    { "en": "Kata Baku Dari 'Pramu Wisma'?", "id": "Pramuwisma" },
    { "en": "Kata Baku Dari 'Prosentase'?", "id": "Persentase" },
    { "en": "Kata Baku Dari 'Seksama'?", "id": "Saksama" },
    { "en": "Kata Baku Dari 'Mempengaruhi'?", "id": "Memengaruhi" },
    { "en": "Kata Baku Dari 'Bokap'?", "id": "Ayah" },
    { "en": "Kata Baku Dari 'Nyokap'?", "id": "Ibu" },
    { "en": "Kata Baku Dari 'Doi'?", "id": "Dia" },
    { "en": "Kata Baku Dari 'Gebetan'?", "id": "Pujaan Hati" },
    { "en": "Kata Baku Dari 'Nongkrong'?", "id": "Berkumpul" },
    { "en": "Kata Baku Dari 'Kece'?", "id": "Keren" },
    { "en": "Kata Baku Dari 'Bokek'?", "id": "Tidak Punya Uang" },
    { "en": "Kata Baku Dari 'Gimana'?", "id": "Bagaimana" },
    { "en": "Kata Baku Dari 'Biarin'?", "id": "Biarkan" },
    { "en": "Kata Baku Dari 'Ngapain'?", "id": "Sedang Apa" },
    { "en": "Kata Baku Dari 'Nggak'?", "id": "Tidak" },
    { "en": "Kata Baku Dari 'Tandatangan'?", "id": "Tanda Tangan" },
    { "en": "Kata Baku Dari 'Saputangan'?", "id": "Sapu Tangan" },
    { "en": "Kata Baku Dari 'Acapkali'?", "id": "Acap Kali" },
    { "en": "Kata Baku Dari 'Ada Kalanya'?", "id": "Adakalanya" },
    { "en": "Kata Baku Dari 'Duka Cita'?", "id": "Dukacita" },
    { "en": "Kata Baku Dari 'Mana Suka'?", "id": "Manasuka" },
    { "en": "Kata Baku Dari 'Pada Hal'?", "id": "Padahal" },
    { "en": "Kata Baku Dari 'Sedia Kala'?", "id": "Sediakala" },
    { "en": "Kata Baku Dari 'Astagfirullah'?", "id": "Astaghfirullah" },
    { "en": "Kata Baku Dari 'Inna Lillahi'?", "id": "Innalillahi" },
    { "en": "Kata Baku Dari 'Masya Allah'?", "id": "Masyaallah" },
    { "en": "Kata Baku Dari 'Subhan Allah'?", "id": "Subhanallah" },
    { "en": "Kata Baku Dari 'Barang Kali'?", "id": "Barangkali" },
    { "en": "Kata Baku Dari 'Darma Bakti'?", "id": "Darmabakti" },
    { "en": "Kata Baku Dari 'Darma Siswa'?", "id": "Darmasiswa" },
    { "en": "Kata Baku Dari 'Kilo Gram'?", "id": "Kilogram" },
    { "en": "Kata Baku Dari 'Sapta Krida'?", "id": "Saptakrida" },
    { "en": "Kata Baku Dari 'Sukasuka'?", "id": "Suka-suka" },
    { "en": "Kata Baku Dari 'Analgetik'?", "id": "Analgesik" },
    { "en": "Kata Baku Dari 'Anastesiologi'?", "id": "Anestesiologi" },
    { "en": "Kata Baku Dari 'Hiperbola'?", "id": "Hiperbol" },
    { "en": "Kata Baku Dari 'Hidrolisa'?", "id": "Hidrolisis" },
    { "en": "Kata Baku Dari 'Cardiology'?", "id": "Kardiologi" },
    { "en": "Kata Baku Dari 'Lekosit'?", "id": "Leukosit" },
    { "en": "Kata Baku Dari 'Ginecology'?", "id": "Ginekologi" },
    { "en": "Kata Baku Dari 'Hatta'?", "id": "Lalu" },
    { "en": "Kata Baku Dari 'Sahaya'?", "id": "Saya" },
    { "en": "Kata Baku Dari 'Galau'?", "id": "Gelisah" },
    { "en": "Kata Baku Dari 'Mager'?", "id": "Malas Bergerak" },
    { "en": "Kata Baku Dari 'Alay'?", "id": "Norak" },
    { "en": "Kata Baku Dari 'Baper'?", "id": "Bawa Perasaan" },
    { "en": "Kata Baku Dari 'Bucin'?", "id": "Budak Cinta" },
    { "en": "Kata Baku Dari 'Curhat'?", "id": "Curahan Hati" },
    { "en": "Kata Baku Dari 'Gabut'?", "id": "Gaji Buta" },
    { "en": "Kata Baku Dari 'Gaje'?", "id": "Tidak Jelas" },
    { "en": "Kata Baku Dari 'Gercep'?", "id": "Gerak Cepat" },
    { "en": "Kata Baku Dari 'Japri'?", "id": "Jalur Pribadi" },
    { "en": "Kata Baku Dari 'Kepo'?", "id": "Rasa Ingin Tahu" },
    { "en": "Kata Baku Dari 'Kudet'?", "id": "Kurang Update" },
    { "en": "Kata Baku Dari 'Lebay'?", "id": "Berlebihan" },
    { "en": "Kata Baku Dari 'Mantul'?", "id": "Mantap Betul" },
    { "en": "Kata Baku Dari 'Pansos'?", "id": "Panjat Sosial" },
    { "en": "Kata Baku Dari 'Pewe'?", "id": "Posisi Enak" },
    { "en": "Kata Baku Dari 'Woles'?", "id": "Santai" },
    { "en": "Kata Baku Dari 'Introvert'?", "id": "Introvert" },
    { "en": "Kata Baku Dari 'Invasi'?", "id": "Invasi" },
    { "en": "Kata Baku Dari 'Investigasi'?", "id": "Investigasi" },
    { "en": "Kata Baku Dari 'Irasional'?", "id": "Irasional" },
    { "en": "Kata Baku Dari 'Ironi'?", "id": "Ironi" },
    { "en": "Kata Baku Dari 'Islam'?", "id": "Islam" },
    { "en": "Kata Baku Dari 'Isolat'?", "id": "Isolat" },
    { "en": "Kata Baku Dari 'Israf'?", "id": "Israf" },
    { "en": "Kata Baku Dari 'Isyarat'?", "id": "Isyarat" },
    { "en": "Kata Baku Dari 'Item'?", "id": "Butir" },
    { "en": "Kata Baku Dari 'Iterasi'?", "id": "Iterasi" },
    { "en": "Kata Baku Dari 'Jabal'?", "id": "Jabal" },
    { "en": "Kata Baku Dari 'Jabar'?", "id": "Jabar" },
    { "en": "Kata Baku Dari 'Jadwal'?", "id": "Jadwal" },
    { "en": "Kata Baku Dari 'Jagad'?", "id": "Jagat" },
    { "en": "Kata Baku Dari 'Jago'?", "id": "Jago" },
    { "en": "Kata Baku Dari 'Jahiliyah'?", "id": "Jahiliah" },
    { "en": "Kata Baku Dari 'Jajar'?", "id": "Jajar" },
    { "en": "Kata Baku Dari 'Jakun'?", "id": "Jakun" },
    { "en": "Kata Baku Dari 'Jala'?", "id": "Jala" },
    { "en": "Kata Baku Dari 'Jalal'?", "id": "Jalal" },
    { "en": "Kata Baku Dari 'Jalan'?", "id": "Jalan" },
    { "en": "Kata Baku Dari 'Jali'?", "id": "Jali" },
    { "en": "Kata Baku Dari 'Jalu'?", "id": "Jalu" },
    { "en": "Kata Baku Dari 'Jam'?", "id": "Jam" },
    { "en": "Kata Baku Dari 'Jambak'?", "id": "Jambak" },
    { "en": "Kata Baku Dari 'Jamban'?", "id": "Jamban" },
    { "en": "Kata Baku Dari 'Jambang'?", "id": "Jambang" },
    { "en": "Kata Baku Dari 'Jatidiri'?", "id": "Jati Diri" },
    { "en": "Kata Baku Dari 'Jongos'?", "id": "Pelayan" },
    { "en": "Kata Baku Dari 'Jurumasak'?", "id": "Juru Masak" },
    { "en": "Kata Baku Dari 'Kabarangin'?", "id": "Kabar Angin" },
    { "en": "Kata Baku Dari 'Kacapembesar'?", "id": "Kaca Pembesar" },
    { "en": "Kata Baku Dari 'Kambinghitam'?", "id": "Kambing Hitam" },
    { "en": "Kata Baku Dari 'Katekisasi'?", "id": "Katekisasi" },
    { "en": "Kata Baku Dari 'Katagoris'?", "id": "Kategoris" },
    { "en": "Kata Baku Dari 'Katholik'?", "id": "Katolik" },
    { "en": "Kata Baku Dari 'Keblinger'?", "id": "Tersesat" },
    { "en": "Kata Baku Dari 'Ke Enam'?", "id": "Keenam" },
    { "en": "Kata Baku Dari 'Kelenger'?", "id": "Kelengar" },
    { "en": "Kata Baku Dari 'Keluyuran'?", "id": "Berkeliaran" },
    { "en": "Kata Baku Dari 'Kendati Pun'?", "id": "Kendatipun" },
    { "en": "Kata Baku Dari 'Kere'?", "id": "Miskin" },
    { "en": "Kata Baku Dari 'Keretaapi'?", "id": "Kereta Api" },
    { "en": "Kata Baku Dari 'Keroncongan'?", "id": "Lapar" },
    { "en": "Kata Baku Dari 'Ke Sampingkan'?", "id": "Kesampingkan" },
    { "en": "Kata Baku Dari 'Hilaf'?", "id": "Khilaf" },
    { "en": "Kata Baku Dari 'Ngicau'?", "id": "Berkicau" },
    { "en": "Kata Baku Dari 'Klaripikasi'?", "id": "Klarifikasi" },
    { "en": "Kata Baku Dari 'Nge-klik'?", "id": "Mengklik" },
    { "en": "Kata Baku Dari 'Club'?", "id": "Klub" },
    { "en": "Kata Baku Dari 'Konseleting'?", "id": "Hubungan Pendek" },
    { "en": "Kata Baku Dari 'Korna'?", "id": "Klakson" },
    { "en": "Kata Baku Dari 'Kram'?", "id": "Kejang" },
    { "en": "Kata Baku Dari 'Kuwalat'?", "id": "Kualat" },
    { "en": "Kata Baku Dari 'Kulon'?", "id": "Barat" },
    { "en": "Kata Baku Dari 'Kumlot'?", "id": "Dengan Pujian" },
    { "en": "Kata Baku Dari 'Custom'?", "id": "Kustom" },
    { "en": "Kata Baku Dari 'Quote'?", "id": "Kutipan" },
    { "en": "Kata Baku Dari 'Lagipula'?", "id": "Lagi Pula" },
    { "en": "Kata Baku Dari 'Lan'?", "id": "Dan" },
    { "en": "Kata Baku Dari 'Landscape'?", "id": "Lanskap" },
    { "en": "Kata Baku Dari 'Launch'?", "id": "Luncurkan" },
    { "en": "Kata Baku Dari 'Lay Out'?", "id": "Tata Letak" },
    { "en": "Kata Baku Dari 'Legging'?", "id": "Leging" },
    { "en": "Kata Baku Dari 'Lemaries'?", "id": "Lemari Es" },
    { "en": "Kata Baku Dari 'Les'?", "id": "Kursus" },
    { "en": "Kata Baku Dari 'Lesbi'?", "id": "Lesbian" },
    { "en": "Kata Baku Dari 'Lever'?", "id": "Hati" },
    { "en": "Kata Baku Dari 'Ngelewatin'?", "id": "Melewati" },
    { "en": "Kata Baku Dari 'Link'?", "id": "Tautan" },
    { "en": "Kata Baku Dari 'Lisen'?", "id": "Lisensi" },
    { "en": "Kata Baku Dari 'Listerik'?", "id": "Listrik" },
    { "en": "Kata Baku Dari 'Lobby'?", "id": "Lobi" },
    { "en": "Kata Baku Dari 'Lockdown'?", "id": "Karantina Wilayah" },
    { "en": "Kata Baku Dari 'Lo'?", "id": "Anda" },
    { "en": "Kata Baku Dari 'Loss'?", "id": "Rugi" },
    { "en": "Kata Baku Dari 'Lotre'?", "id": "Lotere" },
    { "en": "Kata Baku Dari 'Loyo'?", "id": "Lemah" },
    { "en": "Kata Baku Dari 'Lunpia'?", "id": "Lumpia" },
    { "en": "Kata Baku Dari 'Mem-back Up'?", "id": "Mencadangkan" },
    { "en": "Kata Baku Dari 'Mem-phk'?", "id": "Me-phk" },
    { "en": "Kata Baku Dari 'Mensosialisasikan'?", "id": "Menyosialisasikan" },
    { "en": "Kata Baku Dari 'Mempopulerkan'?", "id": "Memopulerkan" },
    { "en": "Kata Baku Dari 'Mentertawakan'?", "id": "Menertawakan" },
    { "en": "Kata Baku Dari 'Mentraktir'?", "id": "Menraktir" },
    { "en": "Kata Baku Dari 'Meteorology'?", "id": "Meteorologi" },
    { "en": "Kata Baku Dari 'Metodik'?", "id": "Metodis" },
    { "en": "Kata Baku Dari 'Mikro Ekonomi'?", "id": "Mikroekonomi" },
    { "en": "Kata Baku Dari 'Min'?", "id": "Minus" },
    { "en": "Kata Baku Dari 'Minyakbumi'?", "id": "Minyak Bumi" },
    { "en": "Kata Baku Dari 'Miras'?", "id": "Minuman Keras" },
    { "en": "Kata Baku Dari 'Miss'?", "id": "Nona" },
    { "en": "Kata Baku Dari 'Moksha'?", "id": "Moksa" },
    { "en": "Kata Baku Dari 'Molor'?", "id": "Tertunda" },
    { "en": "Kata Baku Dari 'Montage'?", "id": "Montase" },
    { "en": "Kata Baku Dari 'Mortaliti'?", "id": "Mortalitas" },
    { "en": "Kata Baku Dari 'Multi Fungsi'?", "id": "Multifungsi" },
    { "en": "Kata Baku Dari 'Multi Lateral'?", "id": "Multilateral" },
    { "en": "Kata Baku Dari 'Multi Level'?", "id": "Multitingkat" },
    { "en": "Kata Baku Dari 'Multy Media'?", "id": "Multimedia" },
    { "en": "Kata Baku Dari 'Musium'?", "id": "Museum" },
    { "en": "Kata Baku Dari 'Mute'?", "id": "Bisukan" },
    { "en": "Kata Baku Dari 'Napsu'?", "id": "Nafsu" },
    { "en": "Kata Baku Dari 'Naudzubillah'?", "id": "Nauzubillah" },
    { "en": "Kata Baku Dari 'Nego'?", "id": "Negosiasi" },
    { "en": "Kata Baku Dari 'Neklace'?", "id": "Kalung" },
    { "en": "Kata Baku Dari 'Neo Kolonialisme'?", "id": "Neokolonialisme" },
    { "en": "Kata Baku Dari 'Nervous'?", "id": "Gugup" },
    { "en": "Kata Baku Dari 'Netizen'?", "id": "Warganet" },
    { "en": "Kata Baku Dari 'Nganggur'?", "id": "Menganggur" },
    { "en": "Kata Baku Dari 'Ngebut'?", "id": "Mengebut" },
    { "en": "Kata Baku Dari 'Ngeles'?", "id": "Berkilah" },
    { "en": "Kata Baku Dari 'Nge-print'?", "id": "Mencetak" },
    { "en": "Kata Baku Dari 'Nge-save'?", "id": "Menyimpan" },
    { "en": "Kata Baku Dari 'Ngoceh'?", "id": "Mengoceh" },
    { "en": "Kata Baku Dari 'Ngompreng'?", "id": "Menumpang" },
    { "en": "Kata Baku Dari 'Ngutang'?", "id": "Berutang" },
    { "en": "Kata Baku Dari 'Nipon'?", "id": "Jepang" },
    { "en": "Kata Baku Dari 'Nir'?", "id": "Tanpa" },
    { "en": "Kata Baku Dari 'Nir Laba'?", "id": "Nirlaba" },
    { "en": "Kata Baku Dari 'Nominatip'?", "id": "Nominatif" },
    { "en": "Kata Baku Dari 'Non Blok'?", "id": "Nonblok" },
    { "en": "Kata Baku Dari 'Non Kolesterol'?", "id": "Nonkolesterol" },
    { "en": "Kata Baku Dari 'Normatip'?", "id": "Normatif" },
    { "en": "Kata Baku Dari 'Nota Bene'?", "id": "Notabene" },
    { "en": "Kata Baku Dari 'Notis'?", "id": "Pemberitahuan" },
    { "en": "Kata Baku Dari 'Oasis'?", "id": "Oase" },
    { "en": "Kata Baku Dari 'Objektifitas'?", "id": "Objektivitas" },
    { "en": "Kata Baku Dari 'Oda'?", "id": "Ode" },
    { "en": "Kata Baku Dari 'Odol'?", "id": "Pasta Gigi" },
    { "en": "Kata Baku Dari 'Offside'?", "id": "Luar Garis" },
    { "en": "Kata Baku Dari 'Oksid'?", "id": "Oksida" },
    { "en": "Kata Baku Dari 'Oktagonal'?", "id": "Oktagonal" },
    { "en": "Kata Baku Dari 'Olahvokal'?", "id": "Olah Vokal" },
    { "en": "Kata Baku Dari 'Ombudsmen'?", "id": "Ombudsman" },
    { "en": "Kata Baku Dari 'Omnivora'?", "id": "Omnivor" },
    { "en": "Kata Baku Dari 'Onde Onde'?", "id": "Onde-onde" },
    { "en": "Kata Baku Dari 'Opa'?", "id": "Kakek" },
    { "en": "Kata Baku Dari 'Ordir'?", "id": "Perintah" },
    { "en": "Kata Baku Dari 'Orek'?", "id": "Orak-arik" },
    { "en": "Kata Baku Dari 'Orang Utan'?", "id": "Orangutan" },
    { "en": "Kata Baku Dari 'Otopsi'?", "id": "Autopsi" },
    { "en": "Kata Baku Dari 'Output'?", "id": "Keluaran" },
    { "en": "Kata Baku Dari 'Outsider'?", "id": "Orang Luar" },
    { "en": "Kata Baku Dari 'Overtake'?", "id": "Menyalip" },
    { "en": "Kata Baku Dari 'Pacul'?", "id": "Cangkul" },
    { "en": "Kata Baku Dari 'Pie'?", "id": "Pai" },
    { "en": "Kata Baku Dari 'Pack'?", "id": "Paket" },
    { "en": "Kata Baku Dari 'Pampers'?", "id": "Popok" },
    { "en": "Kata Baku Dari 'Panasdalam'?", "id": "Panas Dalam" },
    { "en": "Kata Baku Dari 'Panca Warna'?", "id": "Pancawarna" },
    { "en": "Kata Baku Dari 'Mancur'?", "id": "Memancur" },
    { "en": "Kata Baku Dari 'Pansus'?", "id": "Panitia Khusus" },
    { "en": "Kata Baku Dari 'Panteisma'?", "id": "Panteisme" },
    { "en": "Kata Baku Dari 'Pantera'?", "id": "Panter" },
    { "en": "Kata Baku Dari 'Pao'?", "id": "Bakpao" },
    { "en": "Kata Baku Dari 'Papilla'?", "id": "Papila" },
    { "en": "Kata Baku Dari 'Paradiso'?", "id": "Surga" },
    { "en": "Kata Baku Dari 'Paralelogram'?", "id": "Jajar Genjang" },
    { "en": "Kata Baku Dari 'Parasute'?", "id": "Parasut" },
    { "en": "Kata Baku Dari 'Parkeet'?", "id": "Parkit" },
    { "en": "Kata Baku Dari 'Parpol'?", "id": "Partai Politik" },
    { "en": "Kata Baku Dari 'Partisip'?", "id": "Partisipasi" },
    { "en": "Kata Baku Dari 'Paru Paru'?", "id": "Paru-paru" },
    { "en": "Kata Baku Dari 'Pasca Panen'?", "id": "Pascapanen" },
    { "en": "Kata Baku Dari 'Pasen'?", "id": "Pasien" },
    { "en": "Kata Baku Dari 'Paso'?", "id": "Vas Bunga" },
    { "en": "Kata Baku Dari 'Password'?", "id": "Kata Sandi" },
    { "en": "Kata Baku Dari 'Pat Gulipat'?", "id": "Patgulipat" },
    { "en": "Kata Baku Dari 'Patrun'?", "id": "Pola" },
    { "en": "Kata Baku Dari 'Pedopilia'?", "id": "Pedofilia" },
    { "en": "Kata Baku Dari 'Penjantan'?", "id": "Pejantan" },
    { "en": "Kata Baku Dari 'Pelan Pelan'?", "id": "Pelan-pelan" },
    { "en": "Kata Baku Dari 'Pemda'?", "id": "Pemerintah Daerah" },
    { "en": "Kata Baku Dari 'Pepe'?", "id": "Jemur" },
    { "en": "Kata Baku Dari 'Percayadiri'?", "id": "Percaya Diri" },
    { "en": "Kata Baku Dari 'Perifer'?", "id": "Periferal" },
    { "en": "Kata Baku Dari 'Pariwara'?", "id": "Periklanan" },
    { "en": "Kata Baku Dari 'Pestapora'?", "id": "Pesta Pora" },
    { "en": "Kata Baku Dari 'Pesticide'?", "id": "Pestisida" },
    { "en": "Kata Baku Dari 'Petakumpet'?", "id": "Petak Umpet" },
    { "en": "Kata Baku Dari 'Petentengan'?", "id": "Berlagak" },
    { "en": "Kata Baku Dari 'Petrol'?", "id": "Bensin" },
    { "en": "Kata Baku Dari 'Pewarna'?", "id": "Pewarna" },
    { "en": "Kata Baku Dari 'Piala'?", "id": "Piala" },
    { "en": "Kata Baku Dari 'Pianika'?", "id": "Pianika" },
    { "en": "Kata Baku Dari 'Piatu'?", "id": "Piatu" },
    { "en": "Kata Baku Dari 'Picing'?", "id": "Picing" },
    { "en": "Kata Baku Dari 'Pidana'?", "id": "Pidana" },
    { "en": "Kata Baku Dari 'Pigura'?", "id": "Pigura" },
    { "en": "Kata Baku Dari 'Pijar'?", "id": "Pijar" },
    { "en": "Kata Baku Dari 'Pikau'?", "id": "Pikau" },
    { "en": "Kata Baku Dari 'Piket'?", "id": "Piket" },
    { "en": "Kata Baku Dari 'Pikul'?", "id": "Pikul" },
    { "en": "Kata Baku Dari 'Pilau'?", "id": "Pilau" },
    { "en": "Kata Baku Dari 'Pilon'?", "id": "Bodoh" },
    { "en": "Kata Baku Dari 'Pilpres'?", "id": "Pemilihan Presiden" },
    { "en": "Kata Baku Dari 'Pil KB'?", "id": "Pil Keluarga Berencana" },
    { "en": "Kata Baku Dari 'Pimpong'?", "id": "Tenis Meja" },
    { "en": "Kata Baku Dari 'Pin'?", "id": "Pin" },
    { "en": "Kata Baku Dari 'Pindang'?", "id": "Pindang" },
    { "en": "Kata Baku Dari 'Pingit'?", "id": "Pingit" },
    { "en": "Kata Baku Dari 'Pingkal'?", "id": "Pingkal" },
    { "en": "Kata Baku Dari 'Pinjal'?", "id": "Pinjal" },
    { "en": "Kata Baku Dari 'Pionir'?", "id": "Pionir" },
    { "en": "Kata Baku Dari 'Pipa'?", "id": "Pipa" },
    { "en": "Kata Baku Dari 'Pipi'?", "id": "Pipi" },
    { "en": "Kata Baku Dari 'Pipil'?", "id": "Pipil" },
    { "en": "Kata Baku Dari 'Pipis'?", "id": "Buang Air Kecil" },
    { "en": "Kata Baku Dari 'Pirit'?", "id": "Pirit" },
    { "en": "Kata Baku Dari 'Pispot'?", "id": "Pispot" },
    { "en": "Kata Baku Dari 'Pita'?", "id": "Pita" },
    { "en": "Kata Baku Dari 'Pites'?", "id": "Pites" },
    { "en": "Kata Baku Dari 'Pitih'?", "id": "Uang" },
    { "en": "Kata Baku Dari 'Pitur'?", "id": "Fitur" },
    { "en": "Kata Baku Dari 'Plek'?", "id": "Sama Persis" },
    { "en": "Kata Baku Dari 'Plong'?", "id": "Lega" },
    { "en": "Kata Baku Dari 'Plot'?", "id": "Alur" },
    { "en": "Kata Baku Dari 'Pluralisma'?", "id": "Pluralisme" },
    { "en": "Kata Baku Dari 'Podcast'?", "id": "Siniar" },
    { "en": "Kata Baku Dari 'Mojok'?", "id": "Menyudut" },
    { "en": "Kata Baku Dari 'Pol'?", "id": "Penuh" },
    { "en": "Kata Baku Dari 'Polah'?", "id": "Tingkah" },
    { "en": "Kata Baku Dari 'Polen'?", "id": "Serbuk Sari" },
    { "en": "Kata Baku Dari 'Moles'?", "id": "Memoles" },
    { "en": "Kata Baku Dari 'Boon'?", "id": "Bonus" },
    { "en": "Kata Baku Dari 'Fortofolio'?", "id": "Portofolio" },
    { "en": "Kata Baku Dari 'Posture'?", "id": "Postur" },
    { "en": "Kata Baku Dari 'Potasium'?", "id": "Kalium" },
    { "en": "Kata Baku Dari 'Pre Order'?", "id": "Prapesan" },
    { "en": "Kata Baku Dari 'Prestise'?", "id": "Gengsi" },
    { "en": "Kata Baku Dari 'Preventip'?", "id": "Preventif" },
    { "en": "Kata Baku Dari 'Preview'?", "id": "Pratinjau" },
    { "en": "Kata Baku Dari 'Pribahasa'?", "id": "Peribahasa" },
    { "en": "Kata Baku Dari 'Prinsipil'?", "id": "Prinsipial" },
    { "en": "Kata Baku Dari 'Prokem'?", "id": "Bahasa Gaul" },
    { "en": "Kata Baku Dari 'Propeler'?", "id": "Baling-baling" },
    { "en": "Kata Baku Dari 'Protektip'?", "id": "Protektif" },
    { "en": "Kata Baku Dari 'Proxy'?", "id": "Proksi" },
    { "en": "Kata Baku Dari 'Pucet'?", "id": "Pucat" },
    { "en": "Kata Baku Dari 'Puhun'?", "id": "Pohon" },
    { "en": "Kata Baku Dari 'Purbasangka'?", "id": "Prasangka" },
    { "en": "Kata Baku Dari 'Purna Wirawan'?", "id": "Purnawirawan" },
    { "en": "Kata Baku Dari 'Putrimalu'?", "id": "Putri Malu" },
    { "en": "Kata Baku Dari 'Rabiul Awal'?", "id": "Rabiulawal" },
    { "en": "Kata Baku Dari 'Rabiul Akhir'?", "id": "Rabiulakhir" },
    { "en": "Kata Baku Dari 'Radenajeng'?", "id": "Raden Ajeng" },
    { "en": "Kata Baku Dari 'Raja Singa'?", "id": "Sifilis" },
    { "en": "Kata Baku Dari 'Rali'?", "id": "Reli" },
    { "en": "Kata Baku Dari 'Rasionalisir'?", "id": "Rasionalisasi" },
    { "en": "Kata Baku Dari 'Rata Rata'?", "id": "Rata-rata" },
    { "en": "Kata Baku Dari 'Re Asuransi'?", "id": "Reasuransi" },
    { "en": "Kata Baku Dari 'Rempong'?", "id": "Rumit" },
    { "en": "Kata Baku Dari 'Ria'?", "id": "Ria" },
    { "en": "Kata Baku Dari 'Riak'?", "id": "Riak" },
    { "en": "Kata Baku Dari 'Rias'?", "id": "Rias" },
    { "en": "Kata Baku Dari 'Riba'?", "id": "Riba" },
    { "en": "Kata Baku Dari 'Rida'?", "id": "Rida" },
    { "en": "Kata Baku Dari 'Rimba'?", "id": "Rimba" },
    { "en": "Kata Baku Dari 'Rimpang'?", "id": "Rimpang" },
    { "en": "Kata Baku Dari 'Rimpel'?", "id": "Kerut" },
    { "en": "Kata Baku Dari 'Rinai'?", "id": "Rinai" },
    { "en": "Kata Baku Dari 'Rincik'?", "id": "Rincik" },
    { "en": "Kata Baku Dari 'Rindang'?", "id": "Rindang" },
    { "en": "Kata Baku Dari 'Ring'?", "id": "Gelanggang" },
    { "en": "Kata Baku Dari 'Ringkik'?", "id": "Ringkik" },
    { "en": "Kata Baku Dari 'Rinso'?", "id": "Detergen" },
    { "en": "Kata Baku Dari 'Rintih'?", "id": "Rintih" },
    { "en": "Kata Baku Dari 'Rintik'?", "id": "Rintik" },
    { "en": "Kata Baku Dari 'Riset'?", "id": "Riset" },
    { "en": "Kata Baku Dari 'Ritual'?", "id": "Ritual" },
    { "en": "Kata Baku Dari 'Rival'?", "id": "Rival" },
    { "en": "Kata Baku Dari 'Riwil'?", "id": "Rewel" },
    { "en": "Kata Baku Dari 'Roda'?", "id": "Roda" },
    { "en": "Kata Baku Dari 'Roh'?", "id": "Roh" },
    { "en": "Kata Baku Dari 'Rojali'?", "id": "Rokok Jarang Beli" },
    { "en": "Kata Baku Dari 'Roker'?", "id": "Anak Rok" },
    { "en": "Kata Baku Dari 'Rol'?", "id": "Rol" },
    { "en": "Kata Baku Dari 'Roller'?", "id": "Penggulung" },
    { "en": "Kata Baku Dari 'Roman'?", "id": "Roman" },
    { "en": "Kata Baku Dari 'Romansa'?", "id": "Romansa" },
    { "en": "Kata Baku Dari 'Rombeng'?", "id": "Rombeng" },
    { "en": "Kata Baku Dari 'Romboid'?", "id": "Belah Ketupat" },
    { "en": "Kata Baku Dari 'Rompang'?", "id": "Rompang" },
    { "en": "Kata Baku Dari 'Romusha'?", "id": "Pekerja Paksa" },
    { "en": "Kata Baku Dari 'Rongga'?", "id": "Rongga" },
    { "en": "Kata Baku Dari 'Ronggeng'?", "id": "Ronggeng" },
    { "en": "Kata Baku Dari 'Rongrong'?", "id": "Rongrong" },
    { "en": "Kata Baku Dari 'Ronta'?", "id": "Ronta" },
    { "en": "Kata Baku Dari 'Rose'?", "id": "Mawar" },
    { "en": "Kata Baku Dari 'Rosok'?", "id": "Rongsok" },
    { "en": "Kata Baku Dari 'Rotan'?", "id": "Rotan" },
    { "en": "Kata Baku Dari 'Rowan'?", "id": "Rowan" },
    { "en": "Kata Baku Dari 'Rubai'?", "id": "Rubai" },
    { "en": "Kata Baku Dari 'Rubel'?", "id": "Rubel" },
    { "en": "Kata Baku Dari 'Rubik'?", "id": "Rubik" },
    { "en": "Kata Baku Dari 'Rucah'?", "id": "Rucah" },
    { "en": "Kata Baku Dari 'Rudal'?", "id": "Rudal" },
    { "en": "Kata Baku Dari 'Rugi'?", "id": "Rugi" },
    { "en": "Kata Baku Dari 'Rujuk'?", "id": "Rujuk" },
    { "en": "Kata Baku Dari 'Rukhsah'?", "id": "Keringanan" },
    { "en": "Kata Baku Dari 'Ruko'?", "id": "Rumah Toko" },
    { "en": "Kata Baku Dari 'Rukun'?", "id": "Rukun" },
    { "en": "Kata Baku Dari 'Rumbai'?", "id": "Rumbai" },
    { "en": "Kata Baku Dari 'Rumen'?", "id": "Rumen" },
    { "en": "Kata Baku Dari 'Rumpi'?", "id": "Bergosip" },
    { "en": "Kata Baku Dari 'Runcing'?", "id": "Runcing" },
    { "en": "Kata Baku Dari 'Rundung'?", "id": "Rundung" },
    { "en": "Kata Baku Dari 'Rupiah'?", "id": "Rupiah" },
    { "en": "Kata Baku Dari 'Nurunin'?", "id": "Menurunkan" },
    { "en": "Kata Baku Dari 'Turun Temurun'?", "id": "Turun-temurun" },
    { "en": "Kata Baku Dari 'Nusuk'?", "id": "Menusuk" },
    { "en": "Kata Baku Dari 'Nutup'?", "id": "Menutup" },
    { "en": "Kata Baku Dari 'Nuturin'?", "id": "Menasihati" },
    { "en": "Kata Baku Dari 'Ubun Ubun'?", "id": "Ubun-ubun" },
    { "en": "Kata Baku Dari 'Ngucapin'?", "id": "Mengucapkan" },
    { "en": "Kata Baku Dari 'Ngukir'?", "id": "Mengukir" },
    { "en": "Kata Baku Dari 'Ngukur'?", "id": "Mengukur" },
    { "en": "Kata Baku Dari 'Ngulang'?", "id": "Mengulang" },
    { "en": "Kata Baku Dari 'Ulang Alik'?", "id": "Ulang-alik" },
    { "en": "Kata Baku Dari 'Ngulas'?", "id": "Mengulas" },
    { "en": "Kata Baku Dari 'Ulem'?", "id": "Undangan" },
    { "en": "Kata Baku Dari 'Ngumpat'?", "id": "Mengumpat" },
    { "en": "Kata Baku Dari 'Ngunjuk'?", "id": "Mengunjuk" },
    { "en": "Kata Baku Dari 'Ngurai'?", "id": "Mengurai" },
    { "en": "Kata Baku Dari 'Ngurus'?", "id": "Mengurus" },
    { "en": "Kata Baku Dari 'Ngurut'?", "id": "Mengurut" },
    { "en": "Kata Baku Dari 'Ngusap'?", "id": "Mengusap" },
    { "en": "Kata Baku Dari 'Ngusik'?", "id": "Mengusik" },
    { "en": "Kata Baku Dari 'Ngusir'?", "id": "Mengusir" },
    { "en": "Kata Baku Dari 'Ngusulin'?", "id": "Mengusulkan" },
    { "en": "Kata Baku Dari 'Ngusung'?", "id": "Mengusung" },
    { "en": "Kata Baku Dari 'Ngusut'?", "id": "Mengusut" },
    { "en": "Kata Baku Dari 'Sa'at'?", "id": "Saat" },
    { "en": "Kata Baku Dari 'Saban'?", "id": "Setiap" },
    { "en": "Kata Baku Dari 'Saptu'?", "id": "Sabtu" },
    { "en": "Kata Baku Dari 'Nyadap'?", "id": "Menyadap" },
    { "en": "Kata Baku Dari 'Sahaja'?", "id": "Sederhana" },
    { "en": "Kata Baku Dari 'Sahifah'?", "id": "Halaman" },
    { "en": "Kata Baku Dari 'Science'?", "id": "Sains" },
    { "en": "Kata Baku Dari 'Sais'?", "id": "Kusir" },
    { "en": "Kata Baku Dari 'Aza'?", "id": "Saja" },
    { "en": "Kata Baku Dari 'Salad'?", "id": "Selada" },
    { "en": "Kata Baku Dari 'Salip'?", "id": "Salep" },
    { "en": "Kata Baku Dari 'Sami'?", "id": "Sama" },
    { "en": "Kata Baku Dari 'Sampaian'?", "id": "Sampiran" },
    { "en": "Kata Baku Dari 'Kesana'?", "id": "Ke Sana" },
    { "en": "Kata Baku Dari 'Sandang'?", "id": "Pakaian" },
    { "en": "Kata Baku Dari 'Kesandung'?", "id": "Tersandung" },
    { "en": "Kata Baku Dari 'Sang Saka'?", "id": "Sangsaka" },
    { "en": "Kata Baku Dari 'Santap'?", "id": "Makan" },
    { "en": "Kata Baku Dari 'Sopansantun'?", "id": "Sopan Santun" },
    { "en": "Kata Baku Dari 'Saru'?", "id": "Tidak Sopan" },
    { "en": "Kata Baku Dari 'Sarwa'?", "id": "Serba" },
    { "en": "Kata Baku Dari 'Sasmita'?", "id": "Pertanda" },
    { "en": "Kata Baku Dari 'Satpam'?", "id": "Satuan Pengamanan" },
    { "en": "Kata Baku Dari 'Satu Padu'?", "id": "Satupadu" },
    { "en": "Kata Baku Dari 'Sodagar'?", "id": "Saudagar" },
    { "en": "Kata Baku Dari 'Saum'?", "id": "Puasa" },
    { "en": "Kata Baku Dari 'Sawala'?", "id": "Debat" },
    { "en": "Kata Baku Dari 'Sayur Mayur'?", "id": "Sayur-mayur" },
    { "en": "Kata Baku Dari 'Scan'?", "id": "Pindai" },
    { "en": "Kata Baku Dari 'Screenshot'?", "id": "Tangkapan Layar" },
    { "en": "Kata Baku Dari 'Setting'?", "id": "Pengaturan" },
    { "en": "Kata Baku Dari 'Share'?", "id": "Bagikan" },
    { "en": "Kata Baku Dari 'Shopping'?", "id": "Belanja" },
    { "en": "Kata Baku Dari 'Shuttlecock'?", "id": "Kok" },
    { "en": "Kata Baku Dari 'Slide'?", "id": "Salindia" },
    { "en": "Kata Baku Dari 'Slow Motion'?", "id": "Gerak Lambat" },
    { "en": "Kata Baku Dari 'Smartphone'?", "id": "Ponsel Pintar" },
    { "en": "Kata Baku Dari 'Soft Drink'?", "id": "Minuman Ringan" },
    { "en": "Kata Baku Dari 'Software'?", "id": "Perangkat Lunak" },
    { "en": "Kata Baku Dari 'Sound System'?", "id": "Tata Suara" },
    { "en": "Kata Baku Dari 'Stand By'?", "id": "Siaga" },
    { "en": "Kata Baku Dari 'Stapler'?", "id": "Pengokot" },
    { "en": "Kata Baku Dari 'Stir'?", "id": "Kemudi" },
    { "en": "Kata Baku Dari 'Streaming'?", "id": "Strim" },
    { "en": "Kata Baku Dari 'Subtitle'?", "id": "Takarir" },
    { "en": "Kata Baku Dari 'Supplier'?", "id": "Pemasok" },
    { "en": "Kata Baku Dari 'Simpansiur'?", "id": "Simpang Siur" },
    { "en": "Kata Baku Dari 'Nadah'?", "id": "Menadah" },
    { "en": "Kata Baku Dari 'Tageh'?", "id": "Tagih" },
    { "en": "Kata Baku Dari 'Tahajut'?", "id": "Tahajud" },
    { "en": "Kata Baku Dari 'Takabbur'?", "id": "Takabur" },
    { "en": "Kata Baku Dari 'Takel'?", "id": "Kerekan" },
    { "en": "Kata Baku Dari 'Talk'?", "id": "Talk" },
    { "en": "Kata Baku Dari 'Tambalban'?", "id": "Tambal Ban" },
    { "en": "Kata Baku Dari 'Tampah'?", "id": "Niru" },
    { "en": "Kata Baku Dari 'Nempel'?", "id": "Menempel" },
    { "en": "Kata Baku Dari 'Tancapgas'?", "id": "Tancap Gas" },
    { "en": "Kata Baku Dari 'Tandahubung'?", "id": "Tanda Hubung" },
    { "en": "Kata Baku Dari 'Tanggungrenteng'?", "id": "Tanggung Renteng" },
    { "en": "Kata Baku Dari 'Tangsel'?", "id": "Tangerang Selatan" },
    { "en": "Kata Baku Dari 'Tap'?", "id": "Keran" },
    { "en": "Kata Baku Dari 'Tapalbatas'?", "id": "Tapal Batas" },
    { "en": "Kata Baku Dari 'Tarling'?", "id": "Gitar Suling" },
    { "en": "Kata Baku Dari 'Naruh'?", "id": "Menaruh" },
    { "en": "Kata Baku Dari 'Tasbeh'?", "id": "Tasbih" },
    { "en": "Kata Baku Dari 'Tasik'?", "id": "Danau" },
    { "en": "Kata Baku Dari 'Tatarias'?", "id": "Tata Rias" },
    { "en": "Kata Baku Dari 'Tawakkal'?", "id": "Tawakal" },
    { "en": "Kata Baku Dari 'Nawan'?", "id": "Menawan" },
    { "en": "Kata Baku Dari 'Nawar'?", "id": "Menawar" },
    { "en": "Kata Baku Dari 'Nayang'?", "id": "Menayang" },
    { "en": "Kata Baku Dari 'Nebak'?", "id": "Menebak" },
    { "en": "Kata Baku Dari 'Nebang'?", "id": "Menebang" },
    { "en": "Kata Baku Dari 'Nebus'?", "id": "Menebus" },
    { "en": "Kata Baku Dari 'Tedeng Aling Aling'?", "id": "Tedeng Aling-aling" },
    { "en": "Kata Baku Dari 'Neguk'?", "id": "Meneguk" },
    { "en": "Kata Baku Dari 'Nekan'?", "id": "Menekan" },
    { "en": "Kata Baku Dari 'Tekor'?", "id": "Rugi" },
    { "en": "Kata Baku Dari 'Nelan'?", "id": "Menelan" },
    { "en": "Kata Baku Dari 'Telanjang Bulat'?", "id": "Telanjang Bulat" },
    { "en": "Kata Baku Dari 'Telat'?", "id": "Terlambat" },
    { "en": "Kata Baku Dari 'Teler'?", "id": "Mabuk" },
    { "en": "Kata Baku Dari 'Neliti'?", "id": "Meneliti" },
    { "en": "Kata Baku Dari 'Nembak'?", "id": "Menembak" },
    { "en": "Kata Baku Dari 'Nembus'?", "id": "Menembus" },
    { "en": "Kata Baku Dari 'Nempa'?", "id": "Menempa" },
    { "en": "Kata Baku Dari 'Temperatur'?", "id": "Suhu" },
    { "en": "Kata Baku Dari 'Nempuh'?", "id": "Menempuh" },
    { "en": "Kata Baku Dari 'Nempur'?", "id": "Bertempur" },
    { "en": "Kata Baku Dari 'Nemu'?", "id": "Menemukan" },
    { "en": "Kata Baku Dari 'Temu Duga'?", "id": "Wawancara" },
    { "en": "Kata Baku Dari 'Tenagadalam'?", "id": "Tenaga Dalam" },
    { "en": "Kata Baku Dari 'Nendang'?", "id": "Menendang" },
    { "en": "Kata Baku Dari 'Ketengah'?", "id": "Ke Tengah" },
    { "en": "Kata Baku Dari 'Tengil'?", "id": "Menyebalkan" },
    { "en": "Kata Baku Dari 'Nengok'?", "id": "Menengok" },
    { "en": "Kata Baku Dari 'Tengsin'?", "id": "Malu" },
    { "en": "Kata Baku Dari 'Nenteng'?", "id": "Menenteng" },
    { "en": "Kata Baku Dari 'Terangbenderang'?", "id": "Terang Benderang" },
    { "en": "Kata Baku Dari 'Nerobos'?", "id": "Menerobos" },
    { "en": "Kata Baku Dari 'Terumbukarang'?", "id": "Terumbu Karang" },
    { "en": "Kata Baku Dari 'Teruna'?", "id": "Taruna" },
    { "en": "Kata Baku Dari 'Terungku'?", "id": "Penjara" },
    { "en": "Kata Baku Dari 'Thinking'?", "id": "Berpikir" },
    { "en": "Kata Baku Dari 'Nimpuk'?", "id": "Menimpuk" },
    { "en": "Kata Baku Dari 'Timtim'?", "id": "Timor Timur" },
    { "en": "Kata Baku Dari 'Ting Tong'?", "id": "Ting-tong" },
    { "en": "Kata Baku Dari 'Tingwe'?", "id": "Linting Sendiri" },
    { "en": "Kata Baku Dari 'Tinner'?", "id": "Tiner" },
    { "en": "Kata Baku Dari 'Tivi'?", "id": "Televisi" },
    { "en": "Kata Baku Dari 'Toa'?", "id": "Pengeras Suara" },
    { "en": "Kata Baku Dari 'Tolan'?", "id": "Teman" },
    { "en": "Kata Baku Dari 'Tol'?", "id": "Jalan Tol" },
    { "en": "Kata Baku Dari 'Tomboy'?", "id": "Tomboi" },
    { "en": "Kata Baku Dari 'Tonil'?", "id": "Sandiwara" },
    { "en": "Kata Baku Dari 'Top Skor'?", "id": "Topskor" },
    { "en": "Kata Baku Dari 'Toples'?", "id": "Stoples" },
    { "en": "Kata Baku Dari 'Tower'?", "id": "Menara" },
    { "en": "Kata Baku Dari 'Training'?", "id": "Pelatihan" },
    { "en": "Kata Baku Dari 'Trah'?", "id": "Keturunan" },
    { "en": "Kata Baku Dari 'Trekking'?", "id": "Lintas Alam" },
    { "en": "Kata Baku Dari 'Tricycle'?", "id": "Sepeda Roda Tiga" },
    { "en": "Kata Baku Dari 'Tripek'?", "id": "Salah Cetak" },
    { "en": "Kata Baku Dari 'Tuala'?", "id": "Handuk" },
    { "en": "Kata Baku Dari 'Nuang'?", "id": "Menuang" },
    { "en": "Kata Baku Dari 'Tuxedo'?", "id": "Tuksedo" },
    { "en": "Kata Baku Dari 'Tumplek'?", "id": "Tumpah" },
    { "en": "Kata Baku Dari 'Numpu'?", "id": "Menumpu" },
    { "en": "Kata Baku Dari 'Nunjang'?", "id": "Menunjang" },
    { "en": "Kata Baku Dari 'Nuntun'?", "id": "Menuntun" },
    { "en": "Kata Baku Dari 'Tupoksi'?", "id": "Tugas Pokok Dan Fungsi" },
    { "en": "Kata Baku Dari 'Tuslah'?", "id": "Biaya Tambahan" },
    { "en": "Kata Baku Dari 'Tuts'?", "id": "Tombol" },
    { "en": "Kata Baku Dari 'Uanglelah'?", "id": "Uang Lelah" },
    { "en": "Kata Baku Dari 'Ublek'?", "id": "Aduk" },
    { "en": "Kata Baku Dari 'Uburubur'?", "id": "Ubur-ubur" },
    { "en": "Kata Baku Dari 'Ujicoba'?", "id": "Uji Coba" },
    { "en": "Kata Baku Dari 'Uzub'?", "id": "Ujub" },
    { "en": "Kata Baku Dari 'Ujungpangkal'?", "id": "Ujung Pangkal" },
    { "en": "Kata Baku Dari 'Umroh'?", "id": "Umrah" },
    { "en": "Kata Baku Dari 'Undangundang'?", "id": "Undang-undang" },
    { "en": "Kata Baku Dari 'Ngundi'?", "id": "Mengundi" },
    { "en": "Kata Baku Dari 'Ngungkap'?", "id": "Mengungkap" },
    { "en": "Kata Baku Dari 'Ngungkit'?", "id": "Mengungkit" },
    { "en": "Kata Baku Dari 'Ngungsi'?", "id": "Mengungsi" },
    { "en": "Kata Baku Dari 'Unisex'?", "id": "Uniseks" },
    { "en": "Kata Baku Dari 'Unjukrasa'?", "id": "Unjuk Rasa" },
    { "en": "Kata Baku Dari 'Upnormal'?", "id": "Tidak Normal" },
    { "en": "Kata Baku Dari 'Ustadzah'?", "id": "Ustazah" },
    { "en": "Kata Baku Dari 'Utik'?", "id": "Utik" },
    { "en": "Kata Baku Dari 'Utuh'?", "id": "Utuh" },
    { "en": "Kata Baku Dari 'Vagina'?", "id": "Vagina" },
    { "en": "Kata Baku Dari 'Valium'?", "id": "Valium" },
    { "en": "Kata Baku Dari 'Vampir'?", "id": "Vampir" },
    { "en": "Kata Baku Dari 'Vandal'?", "id": "Vandal" },
    { "en": "Kata Baku Dari 'Vandalisme'?", "id": "Vandalisme" },
    { "en": "Kata Baku Dari 'Vanila'?", "id": "Vanila" },
    { "en": "Kata Baku Dari 'Varian'?", "id": "Varian" },
    { "en": "Kata Baku Dari 'Variatif'?", "id": "Variatif" },
    { "en": "Kata Baku Dari 'Varises'?", "id": "Varises" },
    { "en": "Kata Baku Dari 'Vaskular'?", "id": "Vaskular" },
    { "en": "Kata Baku Dari 'Vatikan'?", "id": "Vatikan" },
    { "en": "Kata Baku Dari 'Vegan'?", "id": "Vegan" },
    { "en": "Kata Baku Dari 'Vegetarian'?", "id": "Vegetarian" },
    { "en": "Kata Baku Dari 'Vena'?", "id": "Vena" },
    { "en": "Kata Baku Dari 'Vendor'?", "id": "Vendor" },
    { "en": "Kata Baku Dari 'Ventilasi'?", "id": "Ventilasi" },
    { "en": "Kata Baku Dari 'Venus'?", "id": "Venus" },
    { "en": "Kata Baku Dari 'Verba'?", "id": "Verba" },
    { "en": "Kata Baku Dari 'Verbal'?", "id": "Verbal" },
    { "en": "Kata Baku Dari 'Verdikt'?", "id": "Verdikt" },
    { "en": "Kata Baku Dari 'Vermak'?", "id": "Permak" },
    { "en": "Kata Baku Dari 'Vermilion'?", "id": "Vermilion" },
    { "en": "Kata Baku Dari 'Vertebra'?", "id": "Vertebra" },
    { "en": "Kata Baku Dari 'Vertebrata'?", "id": "Vertebrata" }



        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
