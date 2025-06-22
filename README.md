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


    { "en": "Aceh Tenggara (Aceh)?", "id": "Gunung Leuser." },
    { "en": "Kerinci (Jambi) & Solok Selatan (Sumatera Barat)?", "id": "Gunung Kerinci." },
    { "en": "Lumajang & Malang (Jawa Timur)?", "id": "Gunung Semeru." },
    { "en": "Lombok Timur (Nusa Tenggara Barat)?", "id": "Gunung Rinjani." },
    { "en": "Probolinggo, Pasuruan, Lumajang, & Malang (Jawa Timur)?", "id": "Gunung Bromo." },
    { "en": "Karangasem (Bali)?", "id": "Gunung Agung." },
    { "en": "Sleman (DI Yogyakarta) & Klaten, Boyolali, Magelang (Jawa Tengah)?", "id": "Gunung Merapi." },
    { "en": "Pemalang, Banyumas, Brebes, Tegal, & Purbalingga (Jawa Tengah)?", "id": "Gunung Slamet." },
    { "en": "Magelang, Temanggung, & Wonosobo (Jawa Tengah)?", "id": "Gunung Sumbing." },
    { "en": "Karanganyar (Jawa Tengah) & Ngawi, Magetan (Jawa Timur)?", "id": "Gunung Lawu." },
    { "en": "Magelang, Boyolali, & Semarang (Jawa Tengah)?", "id": "Gunung Merbabu." },
    { "en": "Cianjur, Sukabumi, & Bogor (Jawa Barat)?", "id": "Gunung Gede." },
    { "en": "Cianjur, Sukabumi, & Bogor (Jawa Barat)?", "id": "Gunung Pangrango." },
    { "en": "Kuningan & Majalengka (Jawa Barat)?", "id": "Gunung Ciremai." },
    { "en": "Garut (Jawa Barat)?", "id": "Gunung Papandayan." },
    { "en": "Wonosobo, Batang, Kendal, & Temanggung (Jawa Tengah)?", "id": "Gunung Prau." },
    { "en": "Banyuwangi & Bondowoso (Jawa Timur)?", "id": "Gunung Ijen." },
    { "en": "Banyuwangi, Bondowoso, & Jember (Jawa Timur)?", "id": "Gunung Raung." },
    { "en": "Malang, Pasuruan, & Mojokerto (Jawa Timur)?", "id": "Gunung Arjuno." },
    { "en": "Malang, Pasuruan, & Mojokerto (Jawa Timur)?", "id": "Gunung Welirang." },
    { "en": "Probolinggo, Lumajang, Jember, Bondowoso, & Situbondo (Jawa Timur)?", "id": "Gunung Argopuro." },
    { "en": "Enrekang (Sulawesi Selatan)?", "id": "Gunung Latimojong." },
    { "en": "Dompu & Bima (Nusa Tenggara Barat)?", "id": "Gunung Tambora." },
    { "en": "Ende (Nusa Tenggara Timur)?", "id": "Gunung Kelimutu." },
    { "en": "Karo (Sumatera Utara)?", "id": "Gunung Sinabung." },
    { "en": "Pagar Alam & Lahat (Sumatera Selatan)?", "id": "Gunung Dempo." },
    { "en": "Sukabumi & Bogor (Jawa Barat)?", "id": "Gunung Salak." },
    { "en": "Solok (Sumatera Barat)?", "id": "Gunung Talang." },
    { "en": "Agam & Tanah Datar (Sumatera Barat)?", "id": "Gunung Marapi." },
    { "en": "Garut (Jawa Barat)?", "id": "Gunung Cikuray." },
    { "en": "Tasikmalaya (Jawa Barat)?", "id": "Gunung Galunggung." },
    { "en": "Temanggung & Wonosobo (Jawa Tengah)?", "id": "Gunung Sindoro." },
    { "en": "Semarang (Jawa Tengah)?", "id": "Gunung Ungaran." },
    { "en": "Malang & Blitar (Jawa Timur)?", "id": "Gunung Kawi." },
    { "en": "Malang & Blitar (Jawa Timur)?", "id": "Gunung Butak." },
    { "en": "Batu (Jawa Timur)?", "id": "Gunung Panderman." },
    { "en": "Mojokerto & Pasuruan (Jawa Timur)?", "id": "Gunung Penanggungan." },
    { "en": "Kediri, Blitar, & Malang (Jawa Timur)?", "id": "Gunung Kelud." },
    { "en": "Kediri, Nganjuk, Madiun, Ponorogo, & Tulungagung (Jawa Timur)?", "id": "Gunung Wilis." },
    { "en": "Bangli (Bali)?", "id": "Gunung Batur." },
    { "en": "Bantaeng & Gowa (Sulawesi Selatan)?", "id": "Gunung Lompobattang." },
    { "en": "Gowa (Sulawesi Selatan)?", "id": "Gunung Bawakaraeng." },
    { "en": "Siau Tagulandang Biaro (Sulawesi Utara)?", "id": "Gunung Karangetang." },
    { "en": "Tomohon (Sulawesi Utara)?", "id": "Gunung Lokon." },
    { "en": "Minahasa Tenggara (Sulawesi Utara)?", "id": "Gunung Soputan." },
    { "en": "Ternate (Maluku Utara)?", "id": "Gunung Gamalama." },
    { "en": "Halmahera Barat (Maluku Utara)?", "id": "Gunung Ibu." },
    { "en": "Halmahera Utara (Maluku Utara)?", "id": "Gunung Dukono." },
    { "en": "Maluku Tengah (Maluku)?", "id": "Gunung Binaiya." },
    { "en": "Pandeglang (Banten)?", "id": "Gunung Pulosari." },
    { "en": "Pandeglang (Banten)?", "id": "Gunung Karang." },
    { "en": "Lampung Selatan (Lampung)?", "id": "Gunung Krakatau." },
    { "en": "Bandung Barat & Subang (Jawa Barat)?", "id": "Gunung Tangkuban Perahu." },
    { "en": "Bandung Barat (Jawa Barat)?", "id": "Gunung Burangrang." },
    { "en": "Bandung (Jawa Barat)?", "id": "Gunung Patuha." },
    { "en": "Garut (Jawa Barat)?", "id": "Gunung Guntur." },
    { "en": "Sumedang (Jawa Barat)?", "id": "Gunung Tampomas." },
    { "en": "Kudus, Jepara, & Pati (Jawa Tengah)?", "id": "Gunung Muria." },
    { "en": "Magelang (Jawa Tengah)?", "id": "Gunung Andong." },
    { "en": "Semarang & Magelang (Jawa Tengah)?", "id": "Gunung Telomoyo." },
    { "en": "Situbondo (Jawa Timur)?", "id": "Gunung Baluran." },
    { "en": "Bondowoso & Banyuwangi (Jawa Timur)?", "id": "Gunung Suket." },
    { "en": "Tabanan (Bali)?", "id": "Gunung Batukaru." },
    { "en": "Sikka (Nusa Tenggara Timur)?", "id": "Gunung Egon." },
    { "en": "Ngada (Nusa Tenggara Timur)?", "id": "Gunung Inierie." },
    { "en": "Flores Timur (Nusa Tenggara Timur)?", "id": "Gunung Lewotobi." },
    { "en": "Bolaang Mongondow & Kotamobagu (Sulawesi Utara)?", "id": "Gunung Ambang." },
    { "en": "Minahasa Utara (Sulawesi Utara)?", "id": "Gunung Klabat." },
    { "en": "Mandailing Natal (Sumatera Utara)?", "id": "Gunung Sorikmarapi." },
    { "en": "Karo (Sumatera Utara)?", "id": "Gunung Sibayak." },
    { "en": "Tanah Datar & Padang Pariaman (Sumatera Barat)?", "id": "Gunung Tandikat." },
    { "en": "Agam & Tanah Datar (Sumatera Barat)?", "id": "Gunung Singgalang." },
    { "en": "Pasaman Barat (Sumatera Barat)?", "id": "Gunung Talamau." },
    { "en": "Merangin (Jambi)?", "id": "Gunung Masurai." },
    { "en": "Rejang Lebong (Bengkulu)?", "id": "Gunung Kaba." },
    { "en": "Lampung Barat (Lampung)?", "id": "Gunung Pesagi." },
    { "en": "Lampung Selatan (Lampung)?", "id": "Gunung Rajabasa." },
    { "en": "Tanggamus (Lampung)?", "id": "Gunung Tanggamus." },
    { "en": "Lebak (Banten) & Bogor, Sukabumi (Jawa Barat)?", "id": "Gunung Halimun." },
    { "en": "Purwakarta (Jawa Barat)?", "id": "Gunung Bongkok." },
    { "en": "Mojokerto, Jombang, & Malang (Jawa Timur)?", "id": "Gunung Anjasmoro." },
    { "en": "Nganjuk & Ponorogo (Jawa Timur)?", "id": "Gunung Liman." },
    { "en": "Bangli (Bali)?", "id": "Gunung Abang." },
    { "en": "Alor (Nusa Tenggara Timur)?", "id": "Gunung Sirung." },
    { "en": "Lembata (Nusa Tenggara Timur)?", "id": "Gunung Lewotolo." },
    { "en": "Halmahera Barat (Maluku Utara)?", "id": "Gunung Gamkonora." },
    { "en": "Maluku Tengah (Maluku)?", "id": "Gunung Api Banda." },
    { "en": "Aceh Besar (Aceh)?", "id": "Gunung Seulawah Agam." },
    { "en": "Bener Meriah (Aceh)?", "id": "Gunung Burni Telong." },
    { "en": "Pidie (Aceh)?", "id": "Gunung Peut Sagoe." },
    { "en": "Lima Puluh Kota & Tanah Datar (Sumatera Barat)?", "id": "Gunung Sago." },
    { "en": "Lingga (Kepulauan Riau)?", "id": "Gunung Daik." },
    { "en": "Pesawaran (Lampung)?", "id": "Gunung Betung." },
    { "en": "Bandung & Sumedang (Jawa Barat)?", "id": "Gunung Manglayang." },
    { "en": "Bandung Barat (Jawa Barat)?", "id": "Gunung Bukit Tunggul." },
    { "en": "Wonosobo (Jawa Tengah)?", "id": "Gunung Bisma." },
    { "en": "Banjarnegara & Pekalongan (Jawa Tengah)?", "id": "Gunung Rogojembangan." },
    { "en": "Jember & Bondowoso (Jawa Timur)?", "id": "Gunung Iyang." },
    { "en": "Enrekang (Sulawesi Selatan)?", "id": "Gunung Rantemario." },
    { "en": "Mamasa (Sulawesi Barat)?", "id": "Gunung Gandang Dewata." },
    { "en": "Donggala (Sulawesi Tengah)?", "id": "Gunung Sojol." },
    { "en": "Sigi & Parigi Moutong (Sulawesi Tengah)?", "id": "Gunung Nokilalaki." },
    { "en": "Jayawijaya (Papua Pegunungan)?", "id": "Gunung Puncak Trikora." },
    { "en": "Pegunungan Bintang (Papua Pegunungan)?", "id": "Gunung Puncak Mandala." },
    { "en": "Pegunungan Arfak (Papua Barat)?", "id": "Gunung Arfak." },
    { "en": "Ngada (Nusa Tenggara Timur)?", "id": "Gunung Ebulobo." },
    { "en": "Kepulauan Sangihe (Sulawesi Utara)?", "id": "Gunung Awu." },
    { "en": "Timor Tengah Selatan (Nusa Tenggara Timur)?", "id": "Gunung Mutis." },
    { "en": "Natuna (Kepulauan Riau)?", "id": "Gunung Ranai." },
    { "en": "Tana Toraja (Sulawesi Selatan)?", "id": "Gunung Rantekombola." },
    { "en": "Kolaka Utara (Sulawesi Tenggara)?", "id": "Gunung Mekongga." },
    { "en": "Maluku Tengah (Maluku)?", "id": "Gunung Salahutu." },
    { "en": "Maluku Barat Daya (Maluku)?", "id": "Gunung Kerbau." },
    { "en": "Halmahera Selatan (Maluku Utara)?", "id": "Gunung Sibela." },
    { "en": "Pesawaran (Lampung)?", "id": "Gunung Pesawaran." },
    { "en": "Majalengka (Jawa Barat)?", "id": "Gunung Ciremai (via Apuy)." },
    { "en": "Kuningan (Jawa Barat)?", "id": "Gunung Ciremai (via Palutungan)." },
    { "en": "Probolinggo (Jawa Timur)?", "id": "Gunung Batok." },
    { "en": "Situbondo (Jawa Timur)?", "id": "Gunung Ringgit." },
    { "en": "Flores Timur (Nusa Tenggara Timur)?", "id": "Gunung Ile Boleng." },
    { "en": "Lembata (Nusa Tenggara Timur)?", "id": "Gunung Ile Werung." },
    { "en": "Tomohon (Sulawesi Utara)?", "id": "Gunung Mahawu." },
    { "en": "Minahasa (Sulawesi Utara)?", "id": "Gunung Tondano." },
    { "en": "Mimika (Papua Tengah)?", "id": "Gunung Jaya." },
    { "en": "Mimika (Papua Tengah)?", "id": "Gunung Ngga Pilimsit." },
    { "en": "Tambrauw (Papua Barat Daya)?", "id": "Gunung Tambrauw." },
    { "en": "Bima (Nusa Tenggara Barat)?", "id": "Gunung Sanggar." },
    { "en": "Garut (Jawa Barat)?", "id": "Gunung Talaga Bodas." },
    { "en": "Katingan (Kalimantan Tengah) & Sintang (Kalimantan Barat)?", "id": "Gunung Bukit Raya." },
    { "en": "Kutai Kartanegara (Kalimantan Timur)?", "id": "Gunung Liangpran." },
    { "en": "Hulu Sungai Tengah (Kalimantan Selatan)?", "id": "Gunung Halau-Halau." },
    { "en": "Kayong Utara (Kalimantan Barat)?", "id": "Gunung Palung." },
    { "en": "Mamuju (Sulawesi Barat)?", "id": "Gunung Ganda Dewata." },
    { "en": "Morowali Utara (Sulawesi Tengah)?", "id": "Gunung Tambusisi." },
    { "en": "Luwu Utara (Sulawesi Selatan)?", "id": "Gunung Balease." },
    { "en": "Manggarai (Nusa Tenggara Timur)?", "id": "Gunung Poco Ranaka." },
    { "en": "Manggarai Timur (Nusa Tenggara Timur)?", "id": "Gunung Anak Ranakah." },
    { "en": "Lembata (Nusa Tenggara Timur)?", "id": "Gunung Ile Ape." },
    { "en": "Buru (Maluku)?", "id": "Gunung Kapalatmada." },
    { "en": "Seram Bagian Barat (Maluku)?", "id": "Gunung Sahuwai." },
    { "en": "Pulau Wetar (Maluku)?", "id": "Gunung Api Wetar." },
    { "en": "Pulau Damar (Maluku)?", "id": "Gunung Wurlali." },
    { "en": "Halmahera Selatan (Maluku Utara)?", "id": "Gunung Makian." },
    { "en": "Mimika (Papua Tengah)?", "id": "Gunung Sumantri Brodjonegoro." },
    { "en": "Puncak Jaya (Papua Tengah)?", "id": "Gunung Ngga Pulu." },
    { "en": "Pegunungan Arfak (Papua Barat)?", "id": "Gunung Mebo." },
    { "en": "Bintan (Kepulauan Riau)?", "id": "Gunung Bintan." },
    { "en": "Karimun (Kepulauan Riau)?", "id": "Gunung Jantan." },
    { "en": "Gayo Lues (Aceh)?", "id": "Gunung Perkison." },
    { "en": "Pasaman (Sumatera Barat)?", "id": "Gunung Pasaman." },
    { "en": "Kerinci (Jambi)?", "id": "Gunung Tujuh." },
    { "en": "Kaur (Bengkulu)?", "id": "Gunung Patah." },
    { "en": "Ogan Komering Ulu Selatan (Sumatera Selatan)?", "id": "Gunung Seminung." },
    { "en": "Pandeglang (Banten)?", "id": "Gunung Aseupan." },
    { "en": "Karawang & Cianjur (Jawa Barat)?", "id": "Gunung Sanggabuana." },
    { "en": "Purwakarta (Jawa Barat)?", "id": "Gunung Parang." },
    { "en": "Majalengka (Jawa Barat)?", "id": "Gunung Cakrabuana." },
    { "en": "Ciamis (Jawa Barat)?", "id": "Gunung Sawal." },
    { "en": "Brebes (Jawa Tengah)?", "id": "Gunung Kumbang." },
    { "en": "Pacitan (Jawa Timur)?", "id": "Gunung Limo." },
    { "en": "Jember (Jawa Timur)?", "id": "Gunung Sadeng." },
    { "en": "Jembrana (Bali)?", "id": "Gunung Patas." },
    { "en": "Sumbawa (Nusa Tenggara Barat)?", "id": "Gunung Batu Lanteh." },
    { "en": "Ngada (Nusa Tenggara Timur)?", "id": "Gunung Amburombu." },
    { "en": "Poso (Sulawesi Tengah)?", "id": "Gunung Pompangeo." },
    { "en": "Boalemo (Gorontalo)?", "id": "Gunung Dapi." },
    { "en": "Bone Bolango (Gorontalo)?", "id": "Gunung Tilongkabila." },
    { "en": "Mamuju Tengah (Sulawesi Barat)?", "id": "Gunung Lumut." },
    { "en": "Pulau Biak (Papua)?", "id": "Gunung Sampari." },
    { "en": "Pulau Yapen (Papua)?", "id": "Gunung Wayland." },
    { "en": "Fakfak (Papua Barat)?", "id": "Gunung Mbaham." },
    { "en": "Aceh Tengah (Aceh)?", "id": "Gunung Burni Geureudong." },
    { "en": "Solok Selatan (Sumatera Barat)?", "id": "Gunung Kerinci." },
    { "en": "Langkat (Sumatera Utara)?", "id": "Gunung Leuser." },
    { "en": "Bogor (Jawa Barat)?", "id": "Gunung Kencana." },
    { "en": "Boyolali (Jawa Tengah)?", "id": "Gunung Bibi." },
    { "en": "Lumajang (Jawa Timur)?", "id": "Gunung Lemongan." },
    { "en": "Buleleng (Bali)?", "id": "Gunung Lesung." },
    { "en": "Sumbawa Barat (Nusa Tenggara Barat)?", "id": "Gunung Olet Sangenges." },
    { "en": "Poso (Sulawesi Tengah)?", "id": "Gunung Katopasa." },
    { "en": "Sigi (Sulawesi Tengah)?", "id": "Gunung Ogoamas." },
    { "en": "Tolikara (Papua Pegunungan)?", "id": "Gunung Idenburg." },
    { "en": "Mimika (Papua Tengah)?", "id": "Gunung Carstensz Timur." },
    { "en": "Banjar (Kalimantan Selatan)?", "id": "Gunung Besar." },
    { "en": "Kapuas Hulu (Kalimantan Barat)?", "id": "Gunung Lawit." },
    { "en": "Malinau (Kalimantan Utara)?", "id": "Gunung Batu Jumak." },
    { "en": "Sumba Timur (Nusa Tenggara Timur)?", "id": "Gunung Wanggameti." },
    { "en": "Kepulauan Tanimbar (Maluku)?", "id": "Gunung Lairkamor." },
    { "en": "Pulau Rote (Nusa Tenggara Timur)?", "id": "Gunung Musak." },
    { "en": "Pulau Seram (Maluku)?", "id": "Gunung Totani." },
    { "en": "Pulau Muna (Sulawesi Tenggara)?", "id": "Gunung Lambelu." },
    { "en": "Wonogiri (Jawa Tengah)?", "id": "Gunung Sepikul." },
    { "en": "Lebak (Banten)?", "id": "Gunung Endut." },
    { "en": "Cianjur (Jawa Barat)?", "id": "Gunung Mananggel." },
    { "en": "Garut (Jawa Barat)?", "id": "Gunung Haruman." },
    { "en": "Banjarnegara (Jawa Tengah)?", "id": "Gunung Tampomas (Jawa Tengah)." },
    { "en": "Magetan (Jawa Timur)?", "id": "Gunung Blego." },
    { "en": "Bondowoso (Jawa Timur)?", "id": "Gunung Raung (Puncak Sejati)." },
    { "en": "Sarmi (Papua)?", "id": "Gunung Puncak Foja." },
    { "en": "Jayapura (Papua)?", "id": "Gunung Cyclops." },
    { "en": "Kaimana (Papua Barat)?", "id": "Gunung Kumawa." },
    { "en": "Aceh Selatan (Aceh)?", "id": "Gunung Kembar." },
    { "en": "Tapanuli Selatan (Sumatera Utara)?", "id": "Gunung Lubuk Raya." },
    { "en": "Langkat & Karo (Sumatera Utara)?", "id": "Gunung Sibuatan." },
    { "en": "Kepahiang (Bengkulu)?", "id": "Gunung Daun." },
    { "en": "Musi Rawas Utara (Sumatera Selatan)?", "id": "Gunung Bukit Cogong." },
    { "en": "Sigi (Sulawesi Tengah)?", "id": "Gunung Gawalise." },
    { "en": "Parigi Moutong (Sulawesi Tengah)?", "id": "Gunung Tinombala." },
    { "en": "Banggai (Sulawesi Tengah)?", "id": "Gunung Tompotika." },
    { "en": "Enrekang (Sulawesi Selatan)?", "id": "Gunung Kabentonu." },
    { "en": "Bombana (Sulawesi Tenggara)?", "id": "Gunung Watuwila." },
    { "en": "Barru (Sulawesi Selatan)?", "id": "Gunung Bulusaraung." },
    { "en": "Pangkajene dan Kepulauan (Sulawesi Selatan)?", "id": "Gunung Sorongan." },
    { "en": "Manggarai Barat (Nusa Tenggara Timur)?", "id": "Gunung Mbeliling." },
    { "en": "Dompu (Nusa Tenggara Barat)?", "id": "Gunung Doro Ncanga." },
    { "en": "Malang (Jawa Timur)?", "id": "Gunung Mujur." },
    { "en": "Bojonegoro (Jawa Timur)?", "id": "Gunung Pandan." },
    { "en": "Kendal (Jawa Tengah)?", "id": "Gunung Pagerwatu." },
    { "en": "Bangka (Kepulauan Bangka Belitung)?", "id": "Gunung Maras." },
    { "en": "Samosir (Sumatera Utara)?", "id": "Gunung Pusuk Buhit." },
    { "en": "Nias Selatan (Sumatera Utara)?", "id": "Gunung Lolomatua." },
    { "en": "Tapanuli Utara (Sumatera Utara)?", "id": "Gunung Martimbang." },
    { "en": "Bungo (Jambi)?", "id": "Gunung Pasir." },
    { "en": "Garut & Tasikmalaya (Jawa Barat)?", "id": "Gunung Karacak." },
    { "en": "Boyolali & Klaten (Jawa Tengah)?", "id": "Gunung Merapi (sisi tenggara)." },
    { "en": "Wonogiri & Sukoharjo (Jawa Tengah)?", "id": "Gunung Gajahmungkur." },
    { "en": "Temanggung (Jawa Tengah)?", "id": "Gunung Perahu (sisi selatan)." },
    { "en": "Malang (Jawa Timur)?", "id": "Gunung Banyak." },
    { "en": "Probolinggo & Jember (Jawa Timur)?", "id": "Gunung Puncak Rengganis." },
    { "en": "Jember & Bondowoso (Jawa Timur)?", "id": "Gunung Puncak Hyang." },
    { "en": "Pulau Bawean, Gresik (Jawa Timur)?", "id": "Gunung Besar (Bawean)." },
    { "en": "Alor (Nusa Tenggara Timur)?", "id": "Gunung Kolana." },
    { "en": "Bima (Nusa Tenggara Barat)?", "id": "Gunung Ine Lika." },
    { "en": "Kapuas Hulu (Kalimantan Barat)?", "id": "Gunung Betung (Kalimantan)." },
    { "en": "Nunukan (Kalimantan Utara)?", "id": "Gunung Harun." },
    { "en": "Malinau (Kalimantan Utara)?", "id": "Gunung Makita." },
    { "en": "Tolitoli (Sulawesi Tengah)?", "id": "Gunung Dako." },
    { "en": "Pinrang (Sulawesi Selatan)?", "id": "Gunung Tiroan." },
    { "en": "Luwu Timur (Sulawesi Selatan)?", "id": "Gunung Lumut (Sulawesi)." },
    { "en": "Buton Utara (Sulawesi Tenggara)?", "id": "Gunung Kabaena." },
    { "en": "Ternate (Maluku Utara)?", "id": "Gunung Hiri." },
    { "en": "Pulau Teun (Maluku)?", "id": "Gunung Serawerna." },
    { "en": "Pulau Nila (Maluku)?", "id": "Gunung Nila." },
    { "en": "Yahukimo (Papua Pegunungan)?", "id": "Gunung Puncak Yamin." },
    { "en": "Intan Jaya (Papua Tengah)?", "id": "Gunung Puncak Jaya (sisi utara)." },
    { "en": "Manokwari Selatan (Papua Barat)?", "id": "Gunung Umsini." },
    { "en": "Serdang Bedagai (Sumatera Utara)?", "id": "Gunung Siborongborong." },
    { "en": "Pasuruan (Jawa Timur)?", "id": "Gunung Ringgit (Arjuno)." },
    { "en": "Probolinggo (Jawa Timur)?", "id": "Gunung Kukusan." },
    { "en": "Sigi (Sulawesi Tengah)?", "id": "Gunung Rorekatimbu." },
    { "en": "Nabire (Papua Tengah)?", "id": "Gunung Weyland." },
    { "en": "Sintang (Kalimantan Barat)?", "id": "Gunung Kelam." },
    { "en": "Pemalang (Jawa Tengah)?", "id": "Gunung Pulosari (Jawa Tengah)." },
    { "en": "Banggai Kepulauan (Sulawesi Tengah)?", "id": "Gunung Tambin." },
    { "en": "Kepulauan Mentawai (Sumatera Barat)?", "id": "Gunung Simanuk." },
    { "en": "Aceh Tengah (Aceh)?", "id": "Gunung Bur ni Kelieten." },
    { "en": "Bandung Barat (Jawa Barat)?", "id": "Gunung Patapan." },
    { "en": "Cianjur & Bandung Barat (Jawa Barat)?", "id": "Gunung Masigit." },
    { "en": "Trenggalek (Jawa Timur)?", "id": "Gunung Wilis (sisi selatan)." },
    { "en": "Ponorogo (Jawa Timur)?", "id": "Gunung Bayangkaki." },
    { "en": "Jombang (Jawa Timur)?", "id": "Gunung Pucangan." },
    { "en": "Pulau Timor (Nusa Tenggara Timur)?", "id": "Gunung Lakaan." },
    { "en": "Pulau Pantar, Alor (Nusa Tenggara Timur)?", "id": "Gunung Delaki." },
    { "en": "Sanggau (Kalimantan Barat)?", "id": "Gunung Bawang." },
    { "en": "Pegunungan Bintang (Papua Pegunungan)?", "id": "Gunung Puncak Juliana." },
    { "en": "Teluk Bintuni (Papua Barat)?", "id": "Gunung Weriagar." },
    { "en": "Pulau Simeulue (Aceh)?", "id": "Gunung Sibigo." },
    { "en": "Pakpak Bharat (Sumatera Utara)?", "id": "Gunung Simpon." },
    { "en": "Seluma (Bengkulu)?", "id": "Gunung Seblat." },
    { "en": "Kutai Barat (Kalimantan Timur)?", "id": "Gunung Beriun." },
    { "en": "Luwu (Sulawesi Selatan)?", "id": "Gunung Sinaji." },
    { "en": "Buton (Sulawesi Tenggara)?", "id": "Gunung Wamosa." },
    { "en": "Buru Selatan (Maluku)?", "id": "Gunung Fatu Timao." },
    { "en": "Kepulauan Sula (Maluku Utara)?", "id": "Gunung Fagudu." },
    { "en": "Dogiyai (Papua Tengah)?", "id": "Gunung Dejated." }



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
