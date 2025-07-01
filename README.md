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


  { "en": "Frekuensi?", "id": "Simbol f (satuan Hertz)." },
  { "en": "Arus?", "id": "Simbol I (satuan Ampere)." },
  { "en": "Tegangan?", "id": "Simbol V (satuan Volt)." },
  { "en": "Hambatan?", "id": "Simbol R (satuan Ohm)." },
  { "en": "Kapasitansi?", "id": "Simbol C (satuan Farad)." },
  { "en": "Induktansi?", "id": "Simbol L (satuan Henry)." },
  { "en": "Daya?", "id": "Simbol P (satuan Watt)." },
  { "en": "Energi?", "id": "Simbol W (satuan Joule)." },
  { "en": "Muatan Listrik?", "id": "Simbol Q (satuan Coulomb)." },
  { "en": "Konduktansi?", "id": "Simbol G (satuan Siemens)." },
  { "en": "Impedansi?", "id": "Simbol Z (satuan Ohm)." },
  { "en": "Reaktansi?", "id": "Simbol X (satuan Ohm)." },
  { "en": "Admitansi?", "id": "Simbol Y (satuan Siemens)." },
  { "en": "Susptansi?", "id": "Simbol B (satuan Siemens)." },
  { "en": "Fluks Magnetik?", "id": "Simbol Φ (satuan Weber)." },
  { "en": "Kuat Medan Magnet?", "id": "Simbol H (satuan Ampere per meter)." },
  { "en": "Kerapatan Fluks Magnetik?", "id": "Simbol B (satuan Tesla)." },
  { "en": "Kuat Medan Listrik?", "id": "Simbol E (satuan Volt per meter)." },
  { "en": "Periode?", "id": "Simbol T (satuan detik)." },
  { "en": "Resistivitas?", "id": "Simbol ρ (satuan Ohm-meter)." },
  { "en": "Konduktivitas?", "id": "Simbol σ (satuan Siemens per meter)." },
  { "en": "Permeabilitas?", "id": "Simbol μ (satuan Henry per meter)." },
  { "en": "Permitivitas?", "id": "Simbol ε (satuan Farad per meter)." },
  { "en": "Penguatan (Gain)?", "id": "Simbol G, Av, Ai (tanpa satuan atau dB)." },
  { "en": "Lebar Pita (Bandwidth)?", "id": "Simbol BW (satuan Hertz)." },
  { "en": "Faktor Daya (Power Factor)?", "id": "Simbol PF (tanpa satuan)." },
  { "en": "Efisiensi?", "id": "Simbol η (tanpa satuan atau %)." },
  { "en": "Waktu?", "id": "Simbol t (satuan detik)." },
  { "en": "Panjang?", "id": "Simbol l (satuan meter)." },
  { "en": "Luas?", "id": "Simbol A (satuan meter persegi)." },
  { "en": "Suhu?", "id": "Simbol T (satuan Kelvin atau Celcius)." },
  { "en": "Resistor?", "id": "Simbol R." },
  { "en": "Kapasitor?", "id": "Simbol C." },
  { "en": "Induktor?", "id": "Simbol L." },
  { "en": "Diode?", "id": "Simbol D." },
  { "en": "Transistor Bipolar?", "id": "Simbol Q." },
  { "en": "Transistor Efek Medan (FET)?", "id": "Simbol Q." },
  { "en": "Operational Amplifier (Op-Amp)?", "id": "Simbol U, IC." },
  { "en": "Transformator?", "id": "Simbol T." },
  { "en": "Gerbang AND?", "id": "Simbol & atau ⋅" },
  { "en": "Gerbang OR?", "id": "Simbol + atau ≥1" },
  { "en": "Gerbang NOT (Inverter)?", "id": "Simbol ' atau bar di atas variabel." },
  { "en": "Gerbang NAND?", "id": "Gabungan simbol AND dan NOT." },
  { "en": "Gerbang NOR?", "id": "Gabungan simbol OR dan NOT." },
  { "en": "Gerbang XOR?", "id": "Simbol ⊕ atau =1" },
  { "en": "Gerbang XNOR?", "id": "Gabungan simbol XOR dan NOT." },
  { "en": "Flip-Flop?", "id": "Simbol FF." },
  { "en": "Mikrokontroler?", "id": "Simbol uC atau MCU." },
  { "en": "Integrated Circuit (IC)?", "id": "Simbol U, IC." },
  { "en": "Light Emitting Diode (LED)?", "id": "Simbol D, LED." },
  { "en": "Dioda Zener?", "id": "Simbol ZD." },
  { "en": "Silicon Controlled Rectifier (SCR)?", "id": "Simbol Q, SCR." },
  { "en": "TRIAC?", "id": "Simbol Q, TRIAC." },
  { "en": "DIAC?", "id": "Simbol D, DIAC." },
  { "en": "Motor?", "id": "Simbol M." },
  { "en": "Relay?", "id": "Simbol K, RY." },
  { "en": "Saklar?", "id": "Simbol S, SW." },
  { "en": "Sekering (Fuse)?", "id": "Simbol F." },
  { "en": "Baterai?", "id": "Simbol BT, G, B." },
  { "en": "Sumber Tegangan?", "id": "Simbol V, E." },
  { "en": "Sumber Arus?", "id": "Simbol I." },
  { "en": "Antena?", "id": "Simbol AE, ANT." },
  { "en": "Kristal Osilator?", "id": "Simbol X, XTAL." },
  { "en": "Speaker?", "id": "Simbol SPK, LS." },
  { "en": "Mikrofon?", "id": "Simbol MIC." },
  { "en": "Fotodioda?", "id": "Simbol D (dengan panah cahaya)." },
  { "en": "Fototransistor?", "id": "Simbol Q (dengan panah cahaya)." },
  { "en": "Optocoupler?", "id": "Simbol U, OC." },
  { "en": "Termistor?", "id": "Simbol R, TH." },
  { "en": "Light Dependent Resistor (LDR)?", "id": "Simbol R, LDR." },
  { "en": "Potensiometer?", "id": "Simbol R, POT, VR." },
  { "en": "Dioda Varactor?", "id": "Simbol D, VC." },
  { "en": "Dioda Schottky?", "id": "Simbol D (dengan bentuk S)." },
  { "en": "Ground (Tanah)?", "id": "Simbol GND." },
  { "en": "VCC/VDD?", "id": "Simbol untuk suplai tegangan positif." },
  { "en": "VSS/VEE?", "id": "Simbol untuk suplai tegangan negatif/ground." },
  { "en": "Konektor Jantan (Plug)?", "id": "Simbol P." },
  { "en": "Konektor Betina (Jack)?", "id": "Simbol J." },
  { "en": "Printed Circuit Board?", "id": "Singkatan PCB." },
  { "en": "Heatsink (Pendingin)?", "id": "Tidak ada simbol standar, sering digambarkan." },
  { "en": "Osilator?", "id": "Simbol OSC, G." },
  { "en": "Filter?", "id": "Simbol FL." },
  { "en": "Attenuator (Pelemah)?", "id": "Simbol berupa kotak dengan panah diagonal." },
  { "en": "Amplifier (Penguat)?", "id": "Simbol A, AMP (segitiga)." },
  { "en": "Fasa?", "id": "Simbol φ (satuan derajat atau radian)." },
  { "en": "Siklus Kerja (Duty Cycle)?", "id": "Simbol D, DC (satuan %)." },
  { "en": "Waktu Naik (Rise Time)?", "id": "Simbol tr (satuan detik)." },
  { "en": "Waktu Turun (Fall Time)?", "id": "Simbol tf (satuan detik)." },
  { "en": "Kawat Jumper?", "id": "Simbol berupa garis." },
  { "en": "Titik Sambungan (Node)?", "id": "Simbol berupa titik tebal pada persimpangan garis." },
  { "en": "Terminal?", "id": "Simbol berupa lingkaran kecil di ujung garis." },
  { "en": "Chassis Ground?", "id": "Simbol ground dengan bentuk seperti garpu." },
  { "en": "Digital Ground?", "id": "Simbol ground dengan segitiga bergaris." },
  { "en": "Lampu Pijar?", "id": "Simbol L, LP (lingkaran dengan silang di dalamnya)." },
  { "en": "Buzzer?", "id": "Simbol BZ." },
  { "en": "Voltmeter?", "id": "Simbol V dalam lingkaran." },
  { "en": "Amperemeter?", "id": "Simbol A dalam lingkaran." },
  { "en": "Ohmmeter?", "id": "Simbol Ω dalam lingkaran." },
  { "en": "Galvanometer?", "id": "Simbol G dalam lingkaran." },
  { "en": "Daya Reaktif?", "id": "Simbol Q (satuan Volt-Ampere Reactive atau VAR)." },
  { "en": "Daya Semu?", "id": "Simbol S (satuan Volt-Ampere atau VA)." },
  { "en": "Konstanta Waktu?", "id": "Simbol τ (satuan detik)." },
  { "en": "Frekuensi Sudut?", "id": "Simbol ω (satuan radian per detik)." },
  { "en": "Kecepatan Cahaya?", "id": "Simbol c (satuan meter per detik)." },
  { "en": "Permeabilitas Vakum?", "id": "Simbol µ₀ (satuan Henry per meter)." },
  { "en": "Permitivitas Vakum?", "id": "Simbol ε₀ (satuan Farad per meter)." },
  { "en": "Muatan Elementer?", "id": "Simbol e (satuan Coulomb)." },
  { "en": "Massa Elektron?", "id": "Simbol me (satuan kilogram)." },
  { "en": "Konstanta Planck?", "id": "Simbol h (satuan Joule-detik)." },
  { "en": "Konstanta Boltzmann?", "id": "Simbol k (satuan Joule per Kelvin)." },
  { "en": "Induktansi Bersama?", "id": "Simbol M (satuan Henry)." },
  { "en": "Koefisien Kopling?", "id": "Simbol k (tanpa satuan)." },
  { "en": "Faktor Kualitas?", "id": "Simbol Q (tanpa satuan)." },
  { "en": "Kedalaman Kulit (Skin Depth)?", "id": "Simbol δ (satuan meter)." },
  { "en": "Tegangan Tembus?", "id": "Simbol VBR (satuan Volt)." },
  { "en": "Arus Bocor?", "id": "Simbol I_leak (satuan Ampere)." },
  { "en": "Tegangan Saturasi?", "id": "Simbol VCE(sat) (satuan Volt)." },
  { "en": "Penguatan Arus DC?", "id": "Simbol hFE atau β (tanpa satuan)." },
  { "en": "Produk Lebar Pita Penguatan?", "id": "Simbol GBWP (satuan Hertz)." },
  { "en": "Laju Perubahan Tegangan (Slew Rate)?", "id": "Simbol SR (satuan Volt per mikrodetik)." },
  { "en": "Rasio Penolakan Modus Bersama?", "id": "Simbol CMRR (satuan desibel)." },
  { "en": "Rasio Penolakan Suplai Daya?", "id": "Simbol PSRR (satuan desibel)." },
  { "en": "Impedansi Masukan?", "id": "Simbol Zin (satuan Ohm)." },
  { "en": "Impedansi Keluaran?", "id": "Simbol Zout (satuan Ohm)." },
  { "en": "Desibel?", "id": "Simbol dB (satuan logaritmik)." },
  { "en": "Dioda Terowongan (Tunnel Diode)?", "id": "Simbol D dengan bentuk khusus." },
  { "en": "Dioda PIN?", "id": "Simbol D dengan lapisan 'i' ditunjukkan." },
  { "en": "JFET?", "id": "Simbol Q, JFET." },
  { "en": "MOSFET?", "id": "Simbol Q, MOSFET." },
  { "en": "IGBT?", "id": "Simbol Q, IGBT." },
  { "en": "Gerbang Schmitt Trigger?", "id": "Simbol gerbang logika dengan histeresis di dalamnya." },
  { "en": "Counter (Pencacah)?", "id": "Simbol CTR." },
  { "en": "Register?", "id": "Simbol REG." },
  { "en": "Multiplexer?", "id": "Simbol MUX." },
  { "en": "Demultiplexer?", "id": "Simbol DEMUX." },
  { "en": "Encoder?", "id": "Simbol EN." },
  { "en": "Decoder?", "id": "Simbol DEC." },
  { "en": "Read-Only Memory?", "id": "Simbol ROM." },
  { "en": "Random-Access Memory?", "id": "Simbol RAM." },
  { "en": "CPU (Central Processing Unit)?", "id": "Simbol CPU." },
  { "en": "Digital-to-Analog Converter?", "id": "Simbol DAC." },
  { "en": "Analog-to-Digital Converter?", "id": "Simbol ADC." },
  { "en": "Jembatan Wheatstone?", "id": "Rangkaian 4 resistor dalam bentuk berlian." },
  { "en": "Sumber Tegangan Terkendali Tegangan (VCVS)?", "id": "Simbol sumber dependen berbentuk berlian." },
  { "en": "Sumber Arus Terkendali Tegangan (VCCS)?", "id": "Simbol sumber dependen berbentuk berlian." },
  { "en": "Sumber Tegangan Terkendali Arus (CCVS)?", "id": "Simbol sumber dependen berbentuk berlian." },
  { "en": "Sumber Arus Terkendali Arus (CCCS)?", "id": "Simbol sumber dependen berbentuk berlian." },
  { "en": "Gelombang Sinus?", "id": "Digambarkan sebagai kurva sinus." },
  { "en": "Gelombang Kotak?", "id": "Digambarkan sebagai pulsa-pulsa persegi." },
  { "en": "Gelombang Segitiga?", "id": "Digambarkan sebagai garis lurus naik turun." },
  { "en": "Gelombang Gigi Gergaji?", "id": "Digambarkan seperti gigi gergaji." },
  { "en": "Noise (Derau)?", "id": "Simbol Vn atau In." },
  { "en": "Rasio Sinyal terhadap Derau (SNR)?", "id": "Simbol SNR atau S/N (satuan dB)." },
  { "en": "Distorsi Harmonik Total (THD)?", "id": "Simbol THD (satuan %)." },
  { "en": "Dioda Pemancar Inframerah?", "id": "Singkatan IRED atau IR LED." },
  { "en": "Layar Kristal Cair?", "id": "Singkatan LCD." },
  { "en": "Varistor?", "id": "Simbol VDR (Voltage Dependent Resistor)." },
  { "en": "Konduktor?", "id": "Digambarkan sebagai garis dalam diagram." },
  { "en": "Isolator?", "id": "Merupakan pemisah antar konduktor." },
  { "en": "Semikonduktor?", "id": "Bahan dasar komponen aktif." },
  { "en": "Frekuensi Resonansi?", "id": "Simbol f₀ (satuan Hertz)." },
  { "en": "Polarisasi?", "id": "Menunjukkan orientasi medan gelombang." },
  { "en": "Modulasi Amplitudo?", "id": "Singkatan AM." },
  { "en": "Modulasi Frekuensi?", "id": "Singkatan FM." },
  { "en": "Modulasi Fasa?", "id": "Singkatan PM." },
  { "en": "Kepadatan Arus?", "id": "Simbol J (satuan Ampere per meter persegi)." },
  { "en": "Catu Daya?", "id": "Singkatan PSU (Power Supply Unit)." },
  { "en": "Uninterruptible Power Supply?", "id": "Singkatan UPS." },
  { "en": "Regulator Tegangan?", "id": "Simbol REG, VR." },
  { "en": "Titik Uji (Test Point)?", "id": "Simbol TP." },
  { "en": "Perisai (Shield)?", "id": "Garis putus-putus di sekitar komponen/kabel." },
  { "en": "Kabel Koaksial?", "id": "Simbol khusus menunjukkan konduktor dalam dan perisai." },
  { "en": "Pasangan Terpilin (Twisted Pair)?", "id": "Simbol dua kawat yang saling melilit." },
  { "en": "Pemandu Gelombang (Waveguide)?", "id": "Digambarkan sebagai struktur tabung." },
  { "en": "Sirkulator?", "id": "Simbol lingkaran dengan panah arah aliran sinyal." },
  { "en": "Isolator (Microwave)?", "id": "Simbol mirip sirkulator dengan satu port berbeban." },
  { "en": "Kapasitor Variabel?", "id": "Simbol kapasitor dengan panah menembusnya." },
  { "en": "Induktor Variabel?", "id": "Simbol induktor dengan panah menembusnya." },
  { "en": "Inti Ferit?", "id": "Garis putus-putus di sebelah simbol induktor." },
  { "en": "Inti Besi?", "id": "Garis lurus di sebelah simbol induktor." },
  { "en": "Common Anode Display?", "id": "Tampilan 7-segmen dengan semua anoda terhubung." },
  { "en": "Common Cathode Display?", "id": "Tampilan 7-segmen dengan semua katoda terhubung." },
  { "en": "Arus Maju?", "id": "Simbol IF (satuan Ampere)." },
  { "en": "Tegangan Maju?", "id": "Simbol VF (satuan Volt)." },
  { "en": "Arus Balik?", "id": "Simbol IR (satuan Ampere)." },
  { "en": "Tegangan Balik?", "id": "Simbol VR (satuan Volt)." },
  { "en": "Kapasitansi Sambungan?", "id": "Simbol CJ (satuan Farad)." },
  { "en": "Waktu Pemulihan Balik?", "id": "Simbol trr (satuan detik)." },
  { "en": "Resistansi Termal?", "id": "Simbol Rθ (satuan °C/Watt)." },
  { "en": "Disipasi Daya?", "id": "Simbol PD (satuan Watt)." },
  { "en": "Suhu Sambungan?", "id": "Simbol TJ (satuan °C)." },
  { "en": "Suhu Sekitar?", "id": "Simbol TA (satuan °C)." },
  { "en": "Tegangan Ripple?", "id": "Simbol Vrpp (satuan Volt)." },
  { "en": "Efek Hall?", "id": "Menghasilkan Tegangan Hall (VH) karena medan magnet." },
  { "en": "Magnetoresistansi?", "id": "Perubahan hambatan karena medan magnet." },
  { "en": "Efek Piezoelektrik?", "id": "Menghasilkan tegangan dari tekanan mekanis." },
  { "en": "Efek Seebeck?", "id": "Menghasilkan tegangan dari perbedaan suhu." },
  { "en": "Efek Peltier?", "id": "Menghasilkan perbedaan suhu dari aliran arus." },
  { "en": "Programmable Logic Array (PLA)?", "id": "Blok logika yang dapat diprogram dengan matriks AND dan OR." },
  { "en": "Programmable Array Logic (PAL)?", "id": "Blok logika dengan matriks AND yang dapat diprogram dan matriks OR yang tetap." },
  { "en": "Field-Programmable Gate Array (FPGA)?", "id": "Sirkuit terpadu yang berisi blok logika dan interkoneksi yang dapat dikonfigurasi." },
  { "en": "Complex Programmable Logic Device (CPLD)?", "id": "Perangkat logika terprogram yang lebih kompleks dari PAL tetapi kurang kompleks dari FPGA." },
  { "en": "Application-Specific Integrated Circuit (ASIC)?", "id": "Sirkuit terpadu yang dirancang untuk tujuan atau aplikasi spesifik." },
  { "en": "System on a Chip (SoC)?", "id": "Sirkuit terpadu yang mengintegrasikan semua komponen sistem komputer dalam satu chip." },
  { "en": "Digital Signal Processor (DSP)?", "id": "Mikroprosesor khusus yang dirancang untuk pemrosesan sinyal digital." },
  { "en": "EEPROM (Electrically Erasable Programmable Read-Only Memory)?", "id": "Memori non-volatil yang dapat dihapus dan ditulis ulang secara elektrik." },
  { "en": "Flash Memory?", "id": "Jenis EEPROM yang memungkinkan banyak lokasi memori untuk dihapus atau ditulis dalam satu operasi." },
  { "en": "SRAM (Static Random-Access Memory)?", "id": "Jenis memori volatil yang menyimpan data selama daya menyala tanpa perlu refresh." },
  { "en": "DRAM (Dynamic Random-Access Memory)?", "id": "Jenis memori volatil yang menyimpan bit dalam kapasitor terpisah dan memerlukan refresh periodik." },
  { "en": "SDRAM (Synchronous Dynamic Random-Access Memory)?", "id": "Jenis DRAM yang operasinya disinkronkan dengan clock sistem." },
  { "en": "FIFO (First-In, First-Out)?", "id": "Metode pemrosesan dan antrian data di mana data pertama yang masuk adalah yang pertama keluar." },
  { "en": "LIFO (Last-In, First-Out)?", "id": "Metode antrian data di mana data terakhir yang masuk adalah yang pertama keluar, juga dikenal sebagai tumpukan (stack)." },
  { "en": "Shift Register (Register Geser)?", "id": "Register yang mampu menggeser bit yang disimpannya ke kiri atau kanan." },
  { "en": "Open Collector/Drain Output?", "id": "Jenis keluaran di mana transistor ditarik ke ground tanpa resistor pull-up internal." },
  { "en": "Push-Pull Output?", "id": "Jenis keluaran yang menggunakan dua transistor untuk secara aktif mendorong arus (sourcing) dan menarik arus (sinking)." },
  { "en": "Tri-State Logic?", "id": "Jenis logika yang memiliki tiga keadaan: tinggi (high), rendah (low), dan impedansi tinggi (high-impedance)." },
  { "en": "Tegangan Ambang (Threshold Voltage)?", "id": "Simbol Vth (satuan Volt)." },
  { "en": "Arus Penahan (Holding Current)?", "id": "Simbol IH (satuan Ampere), arus minimum untuk menjaga SCR tetap ON." },
  { "en": "Arus Pengunci (Latching Current)?", "id": "Simbol IL (satuan Ampere), arus minimum untuk menyalakan SCR." },
  { "en": "Gyrator?", "id": "Sirkuit aktif yang mensimulasikan induktor menggunakan kapasitor." },
  { "en": "Negative Impedance Converter (NIC)?", "id": "Sirkuit op-amp yang menghasilkan impedansi negatif." },
  { "en": "Current Mirror (Cermin Arus)?", "id": "Sirkuit yang dirancang untuk menyalin arus melalui satu perangkat aktif dengan mengontrol arus di perangkat lain." },
  { "en": "Penguat Diferensial?", "id": "Penguat yang menguatkan selisih antara dua tegangan masukan." },
  { "en": "Penguat Cascode?", "id": "Konfigurasi dua tahap (misalnya, CE-CB) untuk meningkatkan bandwidth dan impedansi keluaran." },
  { "en": "Penguat Darlington?", "id": "Konfigurasi dua transistor bipolar yang terhubung untuk beroperasi sebagai satu transistor dengan penguatan arus yang sangat tinggi." },
  { "en": "Sensor Efek Hall?", "id": "Transduser yang mengubah medan magnet menjadi tegangan." },
  { "en": "Strain Gauge?", "id": "Sensor yang hambatannya bervariasi sesuai dengan gaya yang diterapkan." },
  { "en": "Accelerometer?", "id": "Sensor yang mengukur percepatan." },
  { "en": "Gyroscope?", "id": "Sensor yang mengukur atau mempertahankan orientasi dan kecepatan sudut." },
  { "en": "Magnetometer?", "id": "Sensor yang mengukur medan magnet atau momen dipol magnetik." },
  { "en": "Sensor Tekanan?", "id": "Sensor yang mengukur tekanan gas atau cairan." },
  { "en": "Sensor Kelembaban?", "id": "Sensor yang mengukur kelembaban relatif." },
  { "en": "Sensor Ultrasonik?", "id": "Sensor yang menggunakan gelombang suara ultrasonik untuk mengukur jarak." },
  { "en": "Sensor Inframerah Pasif (PIR)?", "id": "Sensor elektronik yang mengukur cahaya inframerah dari objek di bidang pandangnya." },
  { "en": "Thermocouple?", "id": "Sensor suhu yang terdiri dari dua konduktor berbeda yang menghasilkan tegangan saat suhunya berubah." },
  { "en": "RTD (Resistance Temperature Detector)?", "id": "Sensor suhu yang hambatannya berubah seiring dengan suhu." },
  { "en": "I2C (Inter-Integrated Circuit)?", "id": "Protokol komunikasi serial dua kabel (SDA, SCL)." },
  { "en": "SPI (Serial Peripheral Interface)?", "id": "Protokol komunikasi serial sinkron empat kabel (MISO, MOSI, SCK, CS)." },
  { "en": "UART (Universal Asynchronous Receiver-Transmitter)?", "id": "Perangkat keras untuk komunikasi serial asinkron (Tx, Rx)." },
  { "en": "CAN Bus (Controller Area Network)?", "id": "Standar bus kendaraan yang dirancang untuk memungkinkan mikrokontroler berkomunikasi satu sama lain." },
  { "en": "Ethernet?", "id": "Teknologi jaringan komputer kabel untuk Local Area Networks (LAN)." },
  { "en": "USB (Universal Serial Bus)?", "id": "Standar industri untuk kabel dan konektor untuk koneksi dan daya antar perangkat." },
  { "en": "RS-232?", "id": "Standar protokol komunikasi serial." },
  { "en": "Kopling Kapasitif?", "id": "Penggunaan kapasitor untuk menghubungkan sinyal AC dari satu bagian sirkuit ke bagian lain sambil memblokir DC." },
  { "en": "Kopling Trafo?", "id": "Penggunaan transformator untuk menghubungkan sinyal, memberikan isolasi galvanik." },
  { "en": "Isolasi Galvanik?", "id": "Prinsip mengisolasi bagian-bagian fungsional dari sistem listrik untuk mencegah aliran arus." },
  { "en": "Kliping (Clipping)?", "id": "Bentuk distorsi yang membatasi puncak sinyal saat melebihi level tertentu." },
  { "en": "Penjepitan (Clamping)?", "id": "Sirkuit yang menggeser level DC sinyal ke level yang diinginkan." },
  { "en": "Rangkaian Snubber?", "id": "Sirkuit yang digunakan untuk menekan lonjakan tegangan atau membatasi laju kenaikan tegangan." },
  { "en": "Crowbar Circuit?", "id": "Sirkuit proteksi yang mencegah kerusakan pada sirkuit lain akibat tegangan berlebih (overvoltage)." },
  { "en": "Pengganda Tegangan?", "id": "Sirkuit listrik yang mengubah tegangan AC rendah menjadi tegangan DC yang lebih tinggi." },
  { "en": "Penyearah Setengah Gelombang?", "id": "Sirkuit penyearah yang hanya melewatkan setengah siklus dari gelombang AC." },
  { "en": "Penyearah Gelombang Penuh?", "id": "Sirkuit penyearah yang mengubah seluruh siklus gelombang AC menjadi DC berdenyut." },
  { "en": "Filter Pi?", "id": "Jenis filter LC yang komponennya menyerupai simbol Yunani Pi (π)." },
  { "en": "Filter T?", "id": "Jenis filter yang komponennya disusun dalam bentuk huruf T." },
  { "en": "Crossover Audio?", "id": "Sirkuit filter yang memisahkan sinyal audio menjadi pita frekuensi yang berbeda untuk speaker yang berbeda." },
  { "en": "Histeresis?", "id": "Ketergantungan keadaan suatu sistem pada riwayatnya, seperti pada Schmitt trigger." },
  { "en": "Tegangan Offset Masukan?", "id": "Simbol Vio (satuan Volt), tegangan DC yang harus diterapkan di antara terminal masukan Op-Amp untuk memaksa keluaran menjadi nol." },
  { "en": "Arus Bias Masukan?", "id": "Simbol Ib (satuan Ampere), arus rata-rata yang mengalir ke terminal masukan Op-Amp." },
  { "en": "Arus Offset Masukan?", "id": "Simbol Iio (satuan Ampere), selisih arus yang mengalir ke terminal masukan Op-Amp." },
  { "en": "Harmonik?", "id": "Gelombang sinus dengan frekuensi yang merupakan kelipatan bilangan bulat dari frekuensi fundamental." },
  { "en": "Intermodulasi?", "id": "Pencampuran dua atau lebih sinyal yang menghasilkan frekuensi baru yang tidak harmonik." },
  { "en": "Konversi Analog ke Digital?", "id": "Proses mengubah sinyal analog kontinu menjadi sinyal digital diskrit." },
  { "en": "Resolusi ADC/DAC?", "id": "Jumlah bit yang digunakan untuk merepresentasikan sinyal (misalnya, 8-bit, 12-bit)." },
  { "en": "Tingkat Pengambilan Sampel (Sample Rate)?", "id": "Jumlah sampel sinyal analog yang diambil per detik (satuan Sampel/detik atau Hz)." },
  { "en": "Kuantisasi?", "id": "Proses memetakan nilai sampel kontinu ke sejumlah level diskrit terbatas." },
  { "en": "Kesalahan Kuantisasi?", "id": "Perbedaan antara sinyal analog asli dan sinyal digital yang dikuantisasi." },
  { "en": "Teorema Nyquist-Shannon?", "id": "Menyatakan bahwa frekuensi pengambilan sampel harus setidaknya dua kali frekuensi tertinggi dalam sinyal." },
  { "en": "Aliasing?", "id": "Efek yang menyebabkan sinyal yang berbeda menjadi tidak dapat dibedakan saat diambil sampelnya." },
  { "en": "Baud Rate?", "id": "Jumlah perubahan simbol, perubahan sinyal, atau peristiwa pensinyalan per detik dalam sinyal yang dimodulasi." },
  { "en": "Bit Rate?", "id": "Jumlah bit yang ditransmisikan atau diproses per satuan waktu (satuan bps)." },
  { "en": "Parity Bit?", "id": "Bit yang ditambahkan ke string data biner untuk memastikan jumlah bit dengan nilai 1 adalah genap atau ganjil." },
  { "en": "Checksum?", "id": "Nilai kecil yang berasal dari blok data digital untuk tujuan mendeteksi kesalahan." },
  { "en": "Siklus Mesin (Machine Cycle)?", "id": "Urutan langkah-langkah yang dilakukan CPU untuk mengeksekusi satu instruksi." },
  { "en": "Set Instruksi?", "id": "Kumpulan perintah yang dapat dimengerti dan dieksekusi oleh CPU." },
  { "en": "Arsitektur Von Neumann?", "id": "Arsitektur komputer yang menggunakan area penyimpanan tunggal untuk instruksi dan data." },
  { "en": "Arsitektur Harvard?", "id": "Arsitektur komputer dengan penyimpanan dan jalur sinyal yang terpisah untuk instruksi dan data." },
  { "en": "Watchdog Timer (WDT)?", "id": "Timer elektronik yang digunakan untuk mendeteksi dan pulih dari kerusakan perangkat lunak komputer." },
  { "en": "DMA (Direct Memory Access)?", "id": "Fitur yang memungkinkan perangkat keras mengakses memori utama secara independen dari CPU." },
  { "en": "Interrupt (Interupsi)?", "id": "Sinyal ke prosesor yang menunjukkan peristiwa yang memerlukan perhatian segera." },
  { "en": "Firmware?", "id": "Perangkat lunak komputer spesifik yang menyediakan kontrol tingkat rendah untuk perangkat keras perangkat." },
  { "en": "Fotolitografi?", "id": "Proses yang digunakan dalam fabrikasi semikonduktor untuk mentransfer pola ke substrat." },
  { "en": "Doping?", "id": "Proses menambahkan pengotor ke semikonduktor intrinsik untuk memodulasi sifat listriknya." },
  { "en": "Wafer Silikon?", "id": "Potongan tipis semikonduktor, seperti silikon kristal, yang digunakan untuk fabrikasi sirkuit terpadu." },
  { "en": "Die (Chip)?", "id": "Blok kecil bahan semikonduktor tempat sirkuit fungsional tertentu dibuat." },
  { "en": "Wire Bonding?", "id": "Metode membuat interkoneksi antara sirkuit terpadu dan perangkat lain." },
  { "en": "Kelas Penguat (A, B, AB, C, D)?", "id": "Klasifikasi yang menggambarkan bagaimana perangkat penguat beroperasi." },
  { "en": "Zero Crossing Detector?", "id": "Sirkuit yang mendeteksi saat sinyal melewati tegangan nol." },
  { "en": "Phase-Locked Loop (PLL)?", "id": "Sistem kontrol yang menghasilkan sinyal keluaran yang fasanya terkait dengan fasa sinyal masukan." },
  { "en": "Voltage-Controlled Oscillator (VCO)?", "id": "Osilator elektronik yang frekuensi osilasinya dikendalikan oleh masukan tegangan." },
  { "en": "Electromagnetic Interference (EMI)?", "id": "Gangguan yang dihasilkan oleh sumber eksternal yang mempengaruhi sirkuit listrik." },
  { "en": "Electromagnetic Compatibility (EMC)?", "id": "Cabang teknik listrik yang berurusan dengan produksi, propagasi, dan penerimaan energi elektromagnetik yang tidak disengaja." },
  { "en": "Electrostatic Discharge (ESD)?", "id": "Pelepasan listrik statis secara tiba-tiba saat dua objek yang diisi listrik bersentuhan." },
  { "en": "Pulse Width Modulation (PWM)?", "id": "Teknik modulasi untuk mengubah lebar pulsa sinyal untuk mengontrol daya atau mengirim data." },
  { "en": "Pulse Code Modulation (PCM)?", "id": "Metode yang digunakan untuk merepresentasikan sinyal analog yang diambil sampelnya secara digital." },
  { "en": "Quadrature Amplitude Modulation (QAM)?", "id": "Skema modulasi digital yang mengirimkan data dengan mengubah amplitudo dari dua gelombang pembawa." },
  { "en": "Phase-Shift Keying (PSK)?", "id": "Skema modulasi digital yang mengirimkan data dengan memodifikasi fasa sinyal pembawa." },
  { "en": "Orthogonal Frequency-Division Multiplexing (OFDM)?", "id": "Metode modulasi digital yang menggunakan sejumlah besar sinyal pembawa ortogonal yang berdekatan." },
  { "en": "Transistor-Transistor Logic (TTL)?", "id": "Keluarga logika digital yang dibangun dari transistor bipolar." },
  { "en": "Complementary Metal-Oxide-Semiconductor (CMOS)?", "id": "Teknologi untuk membuat sirkuit terpadu, dikenal karena konsumsi daya statisnya yang rendah." },
  { "en": "Emitter-Coupled Logic (ECL)?", "id": "Keluarga logika kecepatan tinggi di mana transistor tidak pernah masuk ke saturasi." },
  { "en": "Fan-out?", "id": "Jumlah maksimum masukan gerbang logika yang dapat didorong oleh keluaran gerbang tunggal." },
  { "en": "Fan-in?", "id": "Jumlah masukan yang dimiliki oleh sebuah gerbang logika." },
  { "en": "Tunda Propagasi (Propagation Delay)?", "id": "Simbol tpd, waktu yang dibutuhkan sinyal untuk merambat dari masukan ke keluaran." },
  { "en": "Batas Derau (Noise Margin)?", "id": "Jumlah tegangan derau yang dapat ditoleransi oleh sirkuit tanpa mengubah keadaan keluarannya." },
  { "en": "Waktu Pengaturan (Setup Time)?", "id": "Simbol tsu, waktu minimum di mana sinyal data harus stabil sebelum tepi jam aktif." },
  { "en": "Waktu Penahanan (Hold Time)?", "id": "Simbol th, waktu minimum di mana sinyal data harus stabil setelah tepi jam aktif." },
  { "en": "Metastabilitas?", "id": "Keadaan tidak stabil atau tak terduga dalam sistem digital, sering terjadi karena pelanggaran waktu setup/hold." },
  { "en": "Clock Skew?", "id": "Perbedaan waktu kedatangan sinyal clock di berbagai bagian sirkuit." },
  { "en": "Arithmetic Logic Unit (ALU)?", "id": "Sirkuit digital kombinasional dalam prosesor yang melakukan operasi aritmatika dan bitwise." },
  { "en": "Finite State Machine (FSM)?", "id": "Model komputasi matematis yang terdiri dari sejumlah keadaan (state) dan transisi di antaranya." },
  { "en": "Mealy Machine?", "id": "Jenis FSM di mana outputnya bergantung pada keadaan saat ini dan masukan saat ini." },
  { "en": "Moore Machine?", "id": "Jenis FSM di mana outputnya hanya bergantung pada keadaan saat ini." },
  { "en": "Cache Memory?", "id": "Memori kecil dan cepat yang menyimpan salinan data dari lokasi memori utama yang sering digunakan." },
  { "en": "Pipelining?", "id": "Teknik dalam desain prosesor di mana eksekusi instruksi tumpang tindih dalam beberapa tahap." },
  { "en": "Memory Management Unit (MMU)?", "id": "Komponen perangkat keras komputer yang menangani semua referensi memori (misalnya, translasi alamat virtual ke fisik)." },
  { "en": "Latch-up?", "id": "Jenis hubung singkat dalam sirkuit CMOS yang disebabkan oleh pembentukan struktur SCR parasit." },
  { "en": "Electromigration?", "id": "Transportasi material yang disebabkan oleh pergerakan bertahap ion dalam konduktor karena momentum elektron." },
  { "en": "Daerah Deplesi (Depletion Region)?", "id": "Wilayah dalam semikonduktor di sekitar sambungan p-n yang kekurangan pembawa muatan bebas." },
  { "en": "Lubang (Hole)?", "id": "Ketiadaan elektron di pita valensi; bertindak sebagai pembawa muatan positif." },
  { "en": "Semikonduktor Intrinsik?", "id": "Semikonduktor murni tanpa doping pengotor yang signifikan." },
  { "en": "Semikonduktor Ekstrinsik?", "id": "Semikonduktor yang telah didoping dengan sengaja untuk memodulasi sifat listriknya." },
  { "en": "Etsa (Etching)?", "id": "Proses penghilangan lapisan secara selektif dari permukaan wafer selama fabrikasi." },
  { "en": "Deposisi?", "id": "Proses menumbuhkan atau melapisi bahan tipis ke permukaan wafer." },
  { "en": "S-parameters (Scattering Parameters)?", "id": "Parameter yang digunakan untuk menggambarkan perilaku jaringan listrik linier pada berbagai frekuensi (S11, S21)." },
  { "en": "Return Loss?", "id": "Ukuran daya sinyal yang dipantulkan kembali ke sumbernya karena ketidakcocokan impedansi (satuan dB)." },
  { "en": "Insertion Loss?", "id": "Kehilangan daya sinyal yang diakibatkan oleh penyisipan perangkat dalam jalur transmisi (satuan dB)." },
  { "en": "Smith Chart?", "id": "Grafik nomogram yang digunakan oleh insinyur RF untuk menyelesaikan masalah yang berkaitan dengan saluran transmisi dan rangkaian pencocokan." },
  { "en": "Vector Network Analyzer (VNA)?", "id": "Instrumen yang mengukur parameter jaringan frekuensi radio (seperti S-parameters)." },
  { "en": "Spectrum Analyzer?", "id": "Instrumen yang mengukur besaran sinyal masukan versus frekuensi." },
  { "en": "Logic Analyzer?", "id": "Instrumen yang menangkap dan menampilkan beberapa sinyal dari sistem digital." },
  { "en": "Saluran Transmisi (Transmission Line)?", "id": "Struktur yang dirancang untuk memandu gelombang elektromagnetik (misalnya, kabel koaksial)." },
  { "en": "Impedansi Karakteristik?", "id": "Simbol Z₀ (satuan Ohm), rasio amplitudo tegangan dan arus dari satu gelombang yang merambat di sepanjang saluran." },
  { "en": "Standing Wave Ratio (SWR)?", "id": "Rasio antara amplitudo gelombang berdiri pada maksimum dan minimum; ukuran pencocokan impedansi." },
  { "en": "Microstrip?", "id": "Jenis saluran transmisi planar yang fabrikasinya mudah di atas PCB." },
  { "en": "Stripline?", "id": "Jenis saluran transmisi di mana konduktor datar diapit di antara dua bidang ground paralel." },
  { "en": "Balun?", "id": "Perangkat yang mengubah antara sinyal seimbang (balanced) dan tidak seimbang (unbalanced)." },
  { "en": "Directional Coupler?", "id": "Komponen RF pasif yang menyalin sebagian daya transmisi ke port lain, sering digunakan untuk memantau daya." },
  { "en": "Power Divider?", "id": "Perangkat yang membagi sinyal input menjadi dua atau lebih sinyal output dengan daya lebih rendah." },
  { "en": "Ferrite Bead?", "id": "Komponen pasif yang menekan noise frekuensi tinggi dalam sirkuit elektronik." },
  { "en": "Common Mode Choke?", "id": "Induktor yang digunakan untuk memblokir derau mode bersama sambil melewatkan sinyal mode diferensial." },
  { "en": "Pensinyalan Diferensial?", "id": "Metode pengiriman informasi secara elektrik menggunakan dua sinyal komplementer." },
  { "en": "Pensinyalan Single-Ended?", "id": "Metode pengiriman sinyal di mana satu kabel membawa sinyal tegangan yang bervariasi sementara kabel lainnya terhubung ke tegangan referensi (ground)." },
  { "en": "Gunn Diode?", "id": "Jenis dioda yang menunjukkan efek resistansi diferensial negatif, digunakan dalam osilator frekuensi tinggi." },
  { "en": "IMPATT Diode?", "id": "Dioda semikonduktor daya tinggi yang digunakan dalam aplikasi elektronik frekuensi tinggi dan gelombang mikro." },
  { "en": "Step Recovery Diode?", "id": "Dioda yang menghasilkan pulsa sangat tajam, digunakan dalam generator pulsa dan pengganda frekuensi." },
  { "en": "Surface Mount Device (SMD)?", "id": "Komponen elektronik yang dipasang langsung ke permukaan Papan Sirkuit Cetak (PCB)." },
  { "en": "Through-Hole Technology (THT)?", "id": "Metode pemasangan komponen elektronik yang melibatkan penyisipan kabel melalui lubang yang dibor di PCB." },
  { "en": "Ball Grid Array (BGA)?", "id": "Jenis kemasan pemasangan permukaan yang menggunakan bola solder kecil untuk interkoneksi." },
  { "en": "Quad Flat Package (QFP)?", "id": "Jenis kemasan pemasangan permukaan dengan pin (lead) yang memanjang dari keempat sisi." },
  { "en": "Latensi?", "id": "Ukuran keterlambatan waktu yang dialami dalam suatu sistem (satuan detik)." },
  { "en": "Jitter?", "id": "Penyimpangan dari periodisitas sinyal yang seharusnya periodik (satuan detik atau UI)." },
  { "en": "Derau Fasa (Phase Noise)?", "id": "Representasi domain frekuensi dari fluktuasi acak dalam fasa sinyal." },
  { "en": "Spurious-Free Dynamic Range (SFDR)?", "id": "Rasio kekuatan sinyal fundamental terhadap sinyal palsu terkuat (satuan dB)." },
  { "en": "Effective Number of Bits (ENOB)?", "id": "Ukuran kualitas sinyal yang didigitalkan, menyatakan resolusi ADC ideal yang akan menghasilkan tingkat derau dan distorsi yang sama." },
  { "en": "Flicker Noise (1/f noise)?", "id": "Jenis derau elektronik dengan spektrum frekuensi yang jatuh secara proporsional terhadap 1/f." },
  { "en": "Shot Noise?", "id": "Derau elektronik yang terjadi ketika jumlah partikel diskrit (seperti elektron) yang membawa muatan cukup kecil untuk menimbulkan fluktuasi statistik yang terdeteksi." },
  { "en": "Derau Termal (Thermal Noise)?", "id": "Derau yang dihasilkan oleh agitasi termal dari pembawa muatan di dalam konduktor listrik pada kesetimbangan." },
  { "en": "Bluetooth?", "id": "Standar teknologi nirkabel untuk pertukaran data dalam jarak pendek." },
  { "en": "Wi-Fi?", "id": "Keluarga protokol jaringan nirkabel berdasarkan standar IEEE 802.11, digunakan untuk LAN nirkabel." },
  { "en": "Zigbee?", "id": "Protokol komunikasi nirkabel berdaya rendah, berkecepatan data rendah, yang ditujukan untuk aplikasi kontrol dan sensor." },
  { "en": "NFC (Near-Field Communication)?", "id": "Sekumpulan protokol komunikasi yang memungkinkan dua perangkat elektronik berkomunikasi dengan mendekatkannya dalam jarak beberapa sentimeter." },
  { "en": "RFID (Radio-Frequency Identification)?", "id": "Penggunaan medan elektromagnetik untuk mengidentifikasi dan melacak tag yang melekat pada objek secara otomatis." },
  { "en": "Cathode Ray Tube (CRT)?", "id": "Tabung vakum yang berisi satu atau lebih senapan elektron dan layar berpendar, digunakan dalam layar dan osiloskop yang lebih tua." },
  { "en": "Delta-Sigma Modulation?", "id": "Metode pengkodean sinyal analog menjadi sinyal digital menggunakan oversampling dan noise shaping." },
  { "en": "Antena Yagi-Uda?", "id": "Antena directional yang terdiri dari beberapa elemen paralel dalam satu garis." },
  { "en": "Antena Dipol?", "id": "Jenis antena paling sederhana yang terdiri dari konduktor lurus yang diberi makan di tengah." },
  { "en": "Antena Monopol?", "id": "Jenis antena yang terdiri dari konduktor lurus tunggal yang dipasang di atas bidang ground." },
  { "en": "Antena Patch?", "id": "Jenis antena gelombang mikro berprofil rendah yang dapat dipasang di permukaan datar." },
  { "en": "Catu Daya Mode Sakelar (SMPS)?", "id": "Catu daya elektronik yang menggabungkan regulator pensaklaran untuk mengubah daya listrik secara efisien." },
  { "en": "Regulator Linear?", "id": "Jenis regulator tegangan yang beroperasi dengan terus-menerus membuang daya berlebih sebagai panas." },
  { "en": "Low-Dropout Regulator (LDO)?", "id": "Regulator tegangan linier DC yang dapat beroperasi dengan perbedaan tegangan input-output yang sangat kecil." },
  { "en": "Pompa Muatan (Charge Pump)?", "id": "Jenis konverter DC-ke-DC yang menggunakan kapasitor untuk penyimpanan energi untuk menaikkan atau menurunkan tegangan." },
  { "en": "Kelas X Kapasitor?", "id": "Kapasitor pengaman yang ditempatkan di antara jalur 'line' dan 'neutral' untuk menekan interferensi." },
  { "en": "Kelas Y Kapasitor?", "id": "Kapasitor pengaman yang ditempatkan antara jalur 'line' dan 'chassis ground' untuk menyaring derau mode bersama." },
  { "en": "Koil Rogowski?", "id": "Perangkat listrik untuk mengukur arus bolak-balik (AC) atau pulsa arus berkecepatan tinggi." },
  { "en": "Transduser?", "id": "Perangkat yang mengubah energi dari satu bentuk ke bentuk lainnya (misalnya, mikrofon, accelerometer)." },
  { "en": "Aktuator?", "id": "Komponen mesin yang bertanggung jawab untuk menggerakkan dan mengendalikan mekanisme atau sistem." },
  { "en": "Solenoida?", "id": "Jenis elektromagnet yang tujuannya adalah untuk menghasilkan medan magnet yang terkontrol dari arus listrik." },
  { "en": "Potensiostat?", "id": "Instrumen elektronik yang mengontrol tegangan antara elektroda kerja dan elektroda referensi." },
  { "en": "Galvanostat?", "id": "Perangkat kontrol yang menjaga arus melalui sel elektrolitik pada tingkat yang konstan." },
  { "en": "Rangkaian Terbuka (Open Circuit)?", "id": "Sirkuit listrik yang tidak lengkap sehingga arus tidak dapat mengalir." },
  { "en": "Hubung Singkat (Short Circuit)?", "id": "Koneksi abnormal antara dua node dari sirkuit listrik dengan impedansi yang sangat rendah." },
  { "en": "Hukum Ohm?", "id": "Menyatakan bahwa arus melalui konduktor antara dua titik berbanding lurus dengan tegangan di antara kedua titik tersebut ($V=IR$)." },
  { "en": "Hukum Arus Kirchhoff (KCL)?", "id": "Menyatakan bahwa jumlah aljabar arus dalam sebuah node dari suatu sirkuit adalah nol." },
  { "en": "Hukum Tegangan Kirchhoff (KVL)?", "id": "Menyatakan bahwa jumlah aljabar tegangan di sekitar loop tertutup mana pun adalah nol." },
  { "en": "Teorema Superposisi?", "id": "Menyatakan bahwa untuk sirkuit linier, arus atau tegangan total di setiap cabang adalah jumlah aljabar dari efek yang dihasilkan oleh setiap sumber secara independen." },
  { "en": "Teorema Thevenin?", "id": "Menyatakan bahwa setiap sirkuit linier dapat disederhanakan menjadi satu sumber tegangan (Vth) dengan resistor seri (Rth)." },
  { "en": "Teorema Norton?", "id": "Menyatakan bahwa setiap sirkuit linier dapat disederhanakan menjadi satu sumber arus (In) dengan resistor paralel (Rn)." },
  { "en": "Transfer Daya Maksimum?", "id": "Terjadi ketika impedansi beban sama dengan konjugat kompleks dari impedansi sumber." },
  { "en": "Verilog?", "id": "Bahasa deskripsi perangkat keras (HDL) yang digunakan untuk memodelkan sistem elektronik." },
  { "en": "VHDL?", "id": "Bahasa deskripsi perangkat keras (HDL) lain yang umum digunakan dalam desain sirkuit digital." },
  { "en": "MEMS (Micro-Electro-Mechanical Systems)?", "id": "Teknologi perangkat mikroskopis yang mengintegrasikan elemen mekanik dan listrik." },
  { "en": "Superkonduktivitas?", "id": "Fenomena di mana suatu material memiliki hambatan listrik nol di bawah suhu kritis tertentu." },
  { "en": "Josephson Junction?", "id": "Sambungan antara dua superkonduktor yang dipisahkan oleh lapisan tipis non-superkonduktor, menunjukkan efek kuantum." },
  { "en": "SQUID (Superconducting Quantum Interference Device)?", "id": "Perangkat sangat sensitif untuk mengukur medan magnet, berdasarkan loop superkonduktor yang berisi Josephson junction." },
  { "en": "Spintronics?", "id": "Studi tentang spin intrinsik elektron dan momen magnetiknya, sebagai tambahan dari muatan elektroniknya." },
  { "en": "Quantum Dot?", "id": "Nanokristal semikonduktor yang sangat kecil sehingga menunjukkan sifat mekanika kuantum." },
  { "en": "Graphene?", "id": "Alotrop karbon yang terdiri dari satu lapisan atom yang disusun dalam kisi heksagonal." },
  { "en": "Fonon (Phonon)?", "id": "Kuantum dari getaran vibrasi dalam kisi kristal." },
  { "en": "Plasmon?", "id": "Kuantum dari osilasi plasma, yaitu osilasi kolektif dari lautan elektron bebas." },
  { "en": "Metamaterial?", "id": "Material buatan yang direkayasa untuk memiliki sifat yang tidak ditemukan di alam, seperti indeks bias negatif." },
  { "en": "Photonic Crystal?", "id": "Struktur optik periodik yang dirancang untuk mempengaruhi pergerakan foton." },
  { "en": "Avalanche Photodiode (APD)?", "id": "Fotodioda yang sangat sensitif yang menggunakan efek longsoran (avalanche) untuk mencapai penguatan internal." },
  { "en": "Charge-Coupled Device (CCD)?", "id": "Perangkat untuk pergerakan muatan listrik, sering digunakan sebagai sensor gambar atau register geser." },
  { "en": "CMOS Image Sensor (CIS)?", "id": "Sensor gambar di mana setiap piksel memiliki sirkuit fotodetektor dan penguat sendiri." },
  { "en": "Bolometer?", "id": "Perangkat untuk mengukur daya radiasi elektromagnetik datang melalui pemanasan material yang hambatannya bergantung suhu." },
  { "en": "Time-of-Flight (ToF) Sensor?", "id": "Sensor yang mengukur jarak dengan menghitung waktu yang dibutuhkan oleh sinyal (cahaya atau suara) untuk pergi dan kembali." },
  { "en": "LIDAR (Light Detection and Ranging)?", "id": "Metode penginderaan jauh yang menggunakan pulsa laser untuk mengukur jarak ke Bumi atau target lain." },
  { "en": "Interferometer?", "id": "Instrumen yang menggunakan superposisi gelombang (interferensi) untuk membuat pengukuran presisi." },
  { "en": "Lock-in Amplifier?", "id": "Jenis penguat yang dapat mengekstrak sinyal dengan frekuensi pembawa yang diketahui dari lingkungan yang sangat bising." },
  { "en": "Celah Pita (Band Gap)?", "id": "Simbol Eg (satuan eV), rentang energi dalam padatan di mana tidak ada keadaan elektron yang dapat ada." },
  { "en": "Celah Pita Langsung (Direct Bandgap)?", "id": "Jenis semikonduktor di mana momentum kristal elektron dan lubang sama di puncak pita valensi dan dasar pita konduksi." },
  { "en": "Celah Pita Tak Langsung (Indirect Bandgap)?", "id": "Jenis semikonduktor di mana momentum elektron dan lubang berbeda, memerlukan bantuan fonon untuk rekombinasi." },
  { "en": "Tingkat Fermi (Fermi Level)?", "id": "Permukaan energi potensial termodinamika total untuk elektron di dalam suatu material." },
  { "en": "Fungsi Kerja (Work Function)?", "id": "Energi minimum yang dibutuhkan untuk melepaskan elektron dari permukaan padatan." },
  { "en": "Mobilitas Pembawa Muatan?", "id": "Simbol µ (satuan cm²/(V·s)), ukuran seberapa cepat pembawa muatan (elektron atau lubang) dapat bergerak melalui logam atau semikonduktor." },
  { "en": "Rekombinasi?", "id": "Proses di mana elektron di pita konduksi kembali ke keadaan energi yang lebih rendah (pita valensi), mengakibatkan penghancuran pasangan elektron-lubang." },
  { "en": "Konverter Buck?", "id": "Jenis konverter DC-DC step-down." },
  { "en": "Konverter Boost?", "id": "Jenis konverter DC-DC step-up." },
  { "en": "Konverter Buck-Boost?", "id": "Jenis konverter DC-DC yang dapat menghasilkan tegangan keluaran yang lebih tinggi atau lebih rendah dari tegangan masukan." },
  { "en": "Konverter Cuk?", "id": "Jenis konverter DC-DC yang polaritas keluarannya berlawanan dengan masukannya." },
  { "en": "Konverter Flyback?", "id": "Konverter DC-DC yang menggunakan transformator untuk menyimpan energi dan memberikan isolasi." },
  { "en": "Konverter Full-Bridge?", "id": "Topologi konverter yang menggunakan empat sakelar (transistor) untuk menggerakkan beban." },
  { "en": "Penguat Kelas E?", "id": "Penguat RF pensaklaran berefisiensi tinggi di mana transistor beroperasi sebagai sakelar." },
  { "en": "Penguat Doherty?", "id": "Penguat RF yang menggunakan penguat utama dan penguat puncak untuk mencapai efisiensi tinggi pada tingkat daya mundur." },
  { "en": "Dielectric Resonator Oscillator (DRO)?", "id": "Osilator yang menggunakan 'puck' keramik sebagai elemen resonan untuk menghasilkan sinyal gelombang mikro yang stabil." },
  { "en": "Frequency Synthesizer?", "id": "Sirkuit elektronik yang menghasilkan rentang frekuensi dari satu osilator referensi." },
  { "en": "Direct Digital Synthesizer (DDS)?", "id": "Metode untuk menghasilkan bentuk gelombang analog secara digital dari referensi frekuensi tunggal." },
  { "en": "Diagram Mata (Eye Diagram)?", "id": "Tampilan osiloskop dari sinyal digital yang ditumpuk, digunakan untuk menilai kualitas sinyal." },
  { "en": "Diagram Konstelasi?", "id": "Representasi sinyal yang dimodulasi secara digital, menampilkan posisi titik-titik dalam bidang kompleks." },
  { "en": "Bit Error Rate (BER)?", "id": "Jumlah bit yang diterima dengan kesalahan dibagi dengan jumlah total bit yang ditransfer." },
  { "en": "Error Vector Magnitude (EVM)?", "id": "Ukuran seberapa jauh titik konstelasi menyimpang dari lokasi idealnya." },
  { "en": "Adjacent Channel Power Ratio (ACPR)?", "id": "Ukuran kebocoran daya dari saluran yang ditransmisikan ke saluran yang berdekatan." },
  { "en": "Filter Nyquist?", "id": "Filter yang memenuhi kriteria Nyquist ISI, penting untuk transmisi data bebas interferensi antar-simbol." },
  { "en": "Filter Raised-Cosine?", "id": "Jenis filter yang umum digunakan untuk pembentukan pulsa dalam sistem komunikasi digital." },
  { "en": "Matched Filter?", "id": "Filter linier yang dirancang untuk memaksimalkan rasio sinyal terhadap derau untuk pulsa yang diketahui." },
  { "en": "Filter Kalman?", "id": "Algoritma yang menggunakan serangkaian pengukuran yang diamati dari waktu ke waktu, yang mengandung ketidakpastian statistik, untuk menghasilkan perkiraan variabel yang tidak diketahui." },
  { "en": "Fast Fourier Transform (FFT)?", "id": "Algoritma yang menghitung transformasi Fourier diskrit (DFT) dengan cepat." },
  { "en": "Konvolusi?", "id": "Operasi matematika pada dua fungsi yang menghasilkan fungsi ketiga yang mengekspresikan bagaimana bentuk satu dimodifikasi oleh yang lain." },
  { "en": "Korelasi?", "id": "Ukuran kesamaan dua sinyal sebagai fungsi dari pergeseran waktu yang diterapkan pada salah satunya." },
  { "en": "Respons Impuls?", "id": "Keluaran suatu sistem ketika disajikan dengan sinyal masukan singkat (impuls)." },
  { "en": "Respons Tangga (Step Response)?", "id": "Perilaku keluaran suatu sistem terhadap masukan yang berubah dari nol ke satu dalam waktu yang sangat singkat." },
  { "en": "Plot Bode?", "id": "Grafik respons frekuensi suatu sistem, menampilkan besaran dan fasa secara terpisah." },
  { "en": "Plot Nyquist?", "id": "Grafik respons frekuensi dalam koordinat polar, digunakan untuk menilai stabilitas sistem." },
  { "en": "Plot Kutub-Nol (Pole-Zero Plot)?", "id": "Representasi fungsi transfer dalam bidang-s kompleks, menunjukkan lokasi kutub dan nol." },
  { "en": "Representasi Ruang Keadaan (State-Space)?", "id": "Model matematika dari sistem fisik sebagai satu set masukan, keluaran, dan variabel keadaan." },
  { "en": "Fungsi Alih (Transfer Function)?", "id": "Simbol H(s), rasio transformasi Laplace dari keluaran terhadap masukan suatu sistem." },
  { "en": "Model OSI?", "id": "Model konseptual yang mengkarakterisasi dan menstandarisasi fungsi komunikasi dari sistem telekomunikasi atau komputasi." },
  { "en": "Lapisan Fisik (Physical Layer)?", "id": "Lapisan pertama dalam model OSI, bertanggung jawab untuk transmisi bit mentah." },
  { "en": "Lapisan Tautan Data (Data Link Layer)?", "id": "Lapisan kedua dalam model OSI, menyediakan transfer data node-ke-node." },
  { "en": "Lapisan Jaringan (Network Layer)?", "id": "Lapisan ketiga dalam model OSI, bertanggung jawab untuk pengalamatan dan perutean paket." },
  { "en": "Lapisan Transport (Transport Layer)?", "id": "Lapisan keempat dalam model OSI, menyediakan transfer data ujung-ke-ujung yang andal." },
  { "en": "Krytron?", "id": "Sakelar tabung gas katoda dingin berkecepatan tinggi, sering digunakan untuk memicu detonator." },
  { "en": "Thyratron?", "id": "Jenis tabung gas yang digunakan sebagai sakelar daya terkontrol berkecepatan tinggi." },
  { "en": "Ignitron?", "id": "Jenis penyearah terkontrol yang digunakan dalam aplikasi industri daya tinggi." },
  { "en": "Dekatron?", "id": "Tabung penghitung gas katoda dingin." },
  { "en": "Tabung Nixie?", "id": "Perangkat tampilan elektronik yang menggunakan pelepasan cahaya gas untuk menampilkan angka atau simbol lain." },
  { "en": "Tabung Mata Ajaib (Magic Eye Tube)?", "id": "Tabung vakum yang memberikan indikasi visual dari kekuatan sinyal radio." },
  { "en": "Entropi (dalam teori informasi)?", "id": "Ukuran ketidakpastian atau keacakan dalam sinyal atau data." },
  { "en": "Kapasitas Kanal?", "id": "Tingkat atas di mana informasi dapat ditransmisikan dengan andal melalui saluran komunikasi." },
  { "en": "Kode Hamming?", "id": "Jenis kode koreksi kesalahan linier." },
  { "en": "Kode Reed-Solomon?", "id": "Kode koreksi kesalahan non-biner yang kuat terhadap kesalahan burst." },
  { "en": "Viterbi Algorithm?", "id": "Algoritma pemrograman dinamis untuk menemukan urutan keadaan tersembunyi yang paling mungkin." },
  { "en": "Radiasi Benda Hitam?", "id": "Radiasi elektromagnetik termal dalam atau di sekitar benda dalam kesetimbangan termodinamika dengan lingkungannya." },
  { "en": "Efek Fotolistrik?", "id": "Emisi elektron ketika cahaya mengenai suatu material." },
  { "en": "Hamburan Compton?", "id": "Hamburan tidak elastis foton oleh partikel bermuatan, biasanya elektron." },
  { "en": "Produksi Pasangan?", "id": "Penciptaan partikel subatomik dan antipartikelnya dari foton netral." },
  { "en": "Anihilasi?", "id": "Proses yang terjadi ketika partikel subatomik bertabrakan dengan antipartikelnya masing-masing." },
  { "en": "Bremsstrahlung?", "id": "Radiasi elektromagnetik yang dihasilkan oleh perlambatan partikel bermuatan ketika dibelokkan oleh partikel bermuatan lain." },
  { "en": "Radiasi Cherenkov?", "id": "Radiasi elektromagnetik yang dipancarkan ketika partikel bermuatan melewati medium dielektrik dengan kecepatan lebih besar dari kecepatan fasa cahaya dalam medium tersebut." },
  { "en": "Kuantisasi Fluks Magnetik?", "id": "Kuantisasi fluks magnetik yang melewati loop superkonduktor." },
  { "en": "Efek Hall Kuantum?", "id": "Versi mekanika kuantum dari efek Hall, yang diamati dalam sistem elektron dua dimensi." },
  { "en": "Efek Zeeman?", "id": "Efek membelahnya garis spektral karena adanya medan magnet statis." },
  { "en": "Efek Stark?", "id": "Pergeseran dan pemisahan garis spektral atom dan molekul karena adanya medan listrik eksternal." },
  { "en": "Laser (Light Amplification by Stimulated Emission of Radiation)?", "id": "Perangkat yang memancarkan cahaya melalui proses amplifikasi optik berdasarkan emisi radiasi terstimulasi." },
  { "en": "Maser (Microwave Amplification by Stimulated Emission of Radiation)?", "id": "Perangkat yang menghasilkan gelombang elektromagnetik koheren melalui amplifikasi oleh emisi terstimulasi (pada frekuensi gelombang mikro)." },
  { "en": "Emisi Spontan?", "id": "Proses di mana sumber cahaya kuantum (seperti atom) bertransisi ke keadaan energi yang lebih rendah, menghasilkan foton." },
  { "en": "Emisi Terstimulasi?", "id": "Proses di mana foton yang masuk dari frekuensi tertentu dapat berinteraksi dengan elektron yang tereksitasi, menyebabkannya turun ke tingkat energi yang lebih rendah." },
  { "en": "Inversi Populasi?", "id": "Keadaan di mana sistem (seperti sekelompok atom) ada dalam keadaan di mana lebih banyak anggota berada dalam keadaan tereksitasi daripada dalam keadaan energi yang lebih rendah." },
  { "en": "Rongga Optik (Optical Cavity)?", "id": "Susunan cermin yang membentuk resonator gelombang berdiri untuk gelombang cahaya, komponen kunci dari laser." },
  { "en": "Q-switching?", "id": "Teknik untuk menghasilkan pulsa laser dengan intensitas sangat tinggi dan durasi pendek." },
  { "en": "Mode-locking?", "id": "Teknik dalam optik di mana laser dibuat untuk menghasilkan pulsa cahaya dengan durasi sangat pendek." },
  { "en": "Serat Optik?", "id": "Serat fleksibel dan transparan yang terbuat dari kaca atau plastik untuk mentransmisikan cahaya." },
  { "en": "Penguat Serat (Fiber Amplifier)?", "id": "Penguat optik yang menggunakan serat optik yang didoping untuk menguatkan sinyal optik." },
  { "en": "Sirkulator Optik?", "id": "Komponen serat optik tiga port yang dirancang untuk mengarahkan cahaya dari satu port ke port berikutnya secara berurutan." },
  { "en": "Kisi Bragg Serat (Fiber Bragg Grating)?", "id": "Jenis reflektor Bragg terdistribusi yang dibangun dalam segmen pendek serat optik yang memantulkan panjang gelombang cahaya tertentu." },
  { "en": "Fotodetektor?", "id": "Sensor cahaya atau radiasi elektromagnetik lainnya." },
  { "en": "Sel Surya?", "id": "Perangkat listrik yang mengubah energi cahaya langsung menjadi listrik melalui efek fotovoltaik." },
  { "en": "Efek Fotovoltaik?", "id": "Penciptaan tegangan atau arus listrik dalam suatu material saat terpapar cahaya." },
  { "en": "Topologi Jaringan?", "id": "Susunan elemen-elemen (link, node, dll) dari suatu jaringan komunikasi." },
  { "en": "Protokol?", "id": "Sistem aturan yang memungkinkan dua atau lebih entitas dalam sistem komunikasi untuk berkomunikasi." },
  { "en": "Topologi Bintang (Star)?", "id": "Topologi jaringan di mana setiap node terhubung ke hub atau switch pusat." },
  { "en": "Topologi Bus?", "id": "Topologi jaringan di mana semua node terhubung ke satu kabel pusat (bus)." },
  { "en": "Topologi Cincin (Ring)?", "id": "Topologi jaringan di mana setiap node terhubung persis dengan dua node lain, membentuk jalur sinyal tunggal yang kontinu." },
  { "en": "Topologi Mesh?", "id": "Topologi jaringan di mana node-node saling terhubung satu sama lain, memungkinkan rute ganda." },
  { "en": "Repeater (Pengulang)?", "id": "Perangkat elektronik yang menerima sinyal dan mentransmisikannya kembali pada level yang lebih tinggi atau daya yang lebih tinggi." },
  { "en": "Hub?", "id": "Titik koneksi umum untuk perangkat dalam jaringan, beroperasi di Lapisan Fisik (Layer 1)." },
  { "en": "Bridge (Jembatan)?", "id": "Perangkat yang menghubungkan beberapa segmen jaringan di Lapisan Tautan Data (Layer 2)." },
  { "en": "Switch (Peralih)?", "id": "Perangkat jaringan yang menghubungkan perangkat bersama dalam jaringan komputer dengan menggunakan packet switching." },
  { "en": "Router (Perute)?", "id": "Perangkat jaringan yang meneruskan paket data antar jaringan komputer, beroperasi di Lapisan Jaringan (Layer 3)." },
  { "en": "Gateway (Gerbang)?", "id": "Node jaringan yang berfungsi sebagai titik akses ke jaringan lain." },
  { "en": "Firewall (Tembok Api)?", "id": "Sistem keamanan jaringan yang memantau dan mengontrol lalu lintas jaringan masuk dan keluar berdasarkan aturan keamanan yang telah ditentukan." },
  { "en": "Alamat MAC?", "id": "Pengidentifikasi unik yang ditetapkan untuk antarmuka jaringan untuk komunikasi di lapisan tautan data." },
  { "en": "Alamat IP?", "id": "Label numerik yang ditetapkan untuk setiap perangkat yang terhubung ke jaringan komputer yang menggunakan Protokol Internet untuk komunikasi." },
  { "en": "Subnet Mask?", "id": "Angka 32-bit yang membagi alamat IP menjadi alamat jaringan dan alamat host." },
  { "en": "Protokol TCP (Transmission Control Protocol)?", "id": "Protokol inti dari suite Protokol Internet, menyediakan pengiriman aliran data yang andal dan berurutan." },
  { "en": "Protokol UDP (User Datagram Protocol)?", "id": "Protokol komunikasi yang tidak memerlukan koneksi (connectionless) dan tidak andal, cocok untuk aplikasi yang sensitif terhadap waktu." },
  { "en": "Protokol HTTP (Hypertext Transfer Protocol)?", "id": "Protokol lapisan aplikasi untuk sistem informasi hypermedia terdistribusi dan kolaboratif." },
  { "en": "Protokol FTP (File Transfer Protocol)?", "id": "Protokol jaringan standar yang digunakan untuk transfer file komputer antar klien dan server." },
  { "en": "Protokol SMTP (Simple Mail Transfer Protocol)?", "id": "Standar komunikasi untuk transmisi surat elektronik (email)." },
  { "en": "Protokol DNS (Domain Name System)?", "id": "Sistem penamaan hierarkis dan terdesentralisasi untuk komputer atau sumber daya lain yang terhubung ke Internet." },
  { "en": "Tegangan Mode Bersama (Common-Mode Voltage)?", "id": "Komponen tegangan rata-rata yang sama pada kedua jalur input dari sebuah penguat diferensial." },
  { "en": "Sinyal Mode Bersama (Common-Mode Signal)?", "id": "Sinyal identik yang muncul pada kedua input dari sebuah perangkat diferensial." },
  { "en": "Tegangan Diferensial?", "id": "Perbedaan tegangan antara dua titik." },
  { "en": "Penguat Isolasi?", "id": "Jenis penguat yang dirancang untuk memberikan isolasi galvanik antara sirkuit input dan output." },
  { "en": "Penguat Logaritmik?", "id": "Penguat yang tegangan keluarannya adalah fungsi logaritmik dari tegangan masukannya." },
  { "en": "Penguat Transimpedansi?", "id": "Konverter arus-ke-tegangan, paling sering diimplementasikan dengan op-amp." },
  { "en": "Penguat Terdistribusi?", "id": "Sirkuit yang menggabungkan output dari beberapa perangkat aktif untuk memberikan bandwidth yang lebih lebar daripada perangkat individu." },
  { "en": "Penguat Parametrik?", "id": "Penguat yang sangat sensitif yang menggunakan reaktansi nonlinier (parameter) untuk menguatkan." },
  { "en": "Sample-and-Hold Circuit?", "id": "Sirkuit yang mengambil sampel tegangan dari sinyal analog yang terus berubah dan menahan nilainya pada tingkat konstan untuk periode waktu tertentu." },
  { "en": "Pelindung Faraday (Faraday Cage)?", "id": "Selungkup yang digunakan untuk memblokir medan elektromagnetik." },
  { "en": "Garis Beban DC (DC Load Line)?", "id": "Grafik dari semua kemungkinan kombinasi arus dan tegangan keluaran untuk sebuah transistor dalam rangkaian tertentu." },
  { "en": "Titik Operasi (Q-Point)?", "id": "Titik bias DC dari sebuah transistor, yang ditentukan oleh level VCE dan IC." },
  { "en": "Ayunan Tegangan (Voltage Swing)?", "id": "Rentang maksimum tegangan keluaran dari sebuah penguat." },
  { "en": "Headroom?", "id": "Perbedaan antara level sinyal operasi normal dan level maksimum yang dapat ditangani oleh sistem tanpa distorsi." },
  { "en": "Rangkaian Penjumlahan (Summing Amplifier)?", "id": "Konfigurasi op-amp yang menghasilkan tegangan keluaran yang merupakan jumlah tertimbang dari beberapa tegangan masukan." },
  { "en": "Integrator?", "id": "Sirkuit op-amp yang melakukan operasi integrasi matematika." },
  { "en": "Differentiator?", "id": "Sirkuit op-amp yang melakukan operasi diferensiasi matematika." },
  { "en": "Komparator?", "id": "Perangkat yang membandingkan dua tegangan atau arus dan mengeluarkan sinyal digital yang menunjukkan mana yang lebih besar." },
  { "en": "Multivibrator Astabil?", "id": "Sirkuit elektronik yang tidak memiliki keadaan stabil dan terus-menerus beralih antara dua keadaan, menghasilkan gelombang persegi." },
  { "en": "Multivibrator Monostabil?", "id": "Sirkuit yang memiliki satu keadaan stabil dan satu keadaan tidak stabil. Sebuah pemicu menyebabkannya masuk ke keadaan tidak stabil untuk periode waktu yang ditentukan." },
  { "en": "Multivibrator Bistabil?", "id": "Sirkuit yang memiliki dua keadaan stabil dan dapat dibalik dari satu ke yang lain dengan sinyal pemicu eksternal." },
  { "en": "Kondisi Balapan (Race Condition)?", "id": "Kondisi dalam sistem digital di mana urutan peristiwa yang tidak terduga dapat mempengaruhi output." },
  { "en": "Bahaya Statis (Static Hazard)?", "id": "Kemungkinan kesalahan sementara pada output rangkaian kombinasional karena penundaan propagasi." },
  { "en": "Bahaya Dinamis (Dynamic Hazard)?", "id": "Kemungkinan output berubah lebih dari sekali sebagai hasil dari satu perubahan input." },
  { "en": "Peta Karnaugh?", "id": "Metode untuk menyederhanakan ekspresi aljabar Boolean." },
  { "en": "Ekspansi Shannon?", "id": "Metode untuk memecah fungsi Boolean menjadi sub-fungsi yang lebih kecil." },
  { "en": "Sintesis Logika?", "id": "Proses mengubah deskripsi perilaku sirkuit digital (misalnya, Verilog) menjadi implementasi gerbang logika." },
  { "en": "Verifikasi Formal?", "id": "Penggunaan metode matematika untuk membuktikan atau menyangkal kebenaran suatu sistem sehubungan dengan spesifikasi formal." },
  { "en": "Emulasi?", "id": "Proses meniru perilaku perangkat keras dengan perangkat keras lain, seringkali untuk tujuan pengujian." },
  { "en": "Simulasi?", "id": "Proses meniru perilaku perangkat keras atau sistem menggunakan model perangkat lunak." },
  { "en": "Backplane?", "id": "Sekelompok konektor listrik paralel sehingga setiap pin dari setiap konektor terhubung ke pin yang sama dari semua konektor lainnya." },
  { "en": "Mezzanine Card?", "id": "Papan sirkuit yang dipasang secara paralel dengan papan utama untuk menyediakan fungsionalitas tambahan." },
  { "en": "Konektor Tepi (Edge Connector)?", "id": "Bagian dari papan sirkuit cetak yang terdiri dari jejak yang mengarah ke tepi papan untuk dihubungkan ke soket yang cocok." },
  { "en": "Zero Insertion Force (ZIF) Socket?", "id": "Jenis soket IC yang memungkinkan penyisipan dan pelepasan chip tanpa memerlukan gaya." },
  { "en": "Solder Mask?", "id": "Lapisan polimer tipis yang diaplikasikan pada jejak tembaga papan sirkuit cetak untuk perlindungan terhadap oksidasi dan untuk mencegah jembatan solder." },
  { "en": "Silkscreen?", "id": "Lapisan atas tinta pada PCB yang digunakan untuk mengidentifikasi komponen, titik uji, dan informasi lainnya." },
  { "en": "Via (Vertical Interconnect Access)?", "id": "Sambungan listrik antara lapisan-lapisan papan sirkuit cetak." },
  { "en": "Jejak (Trace)?", "id": "Jalur konduktif pada PCB untuk sinyal listrik." },
  { "en": "Pad?", "id": "Area logam pada PCB di mana komponen disolder." },
  { "en": "Panelisasi?", "id": "Proses membuat beberapa PCB dalam satu papan yang lebih besar (panel) untuk efisiensi manufaktur." },
  { "en": "Reflow Soldering?", "id": "Proses di mana pasta solder digunakan untuk memasang komponen pemasangan permukaan ke PCB, diikuti dengan pemanasan terkontrol untuk melelehkan solder." },
  { "en": "Wave Soldering?", "id": "Proses penyolderan massal di mana PCB dilewatkan di atas panci solder cair, menciptakan gelombang solder yang melapisi komponen." },
  { "en": "Konduksi?", "id": "Perpindahan panas atau listrik melalui kontak langsung." },
  { "en": "Konveksi?", "id": "Perpindahan panas melalui pergerakan fluida (cairan atau gas)." },
  { "en": "Radiasi?", "id": "Emisi energi sebagai gelombang elektromagnetik atau sebagai partikel subatom yang bergerak." },
  { "en": "Koefisien Suhu?", "id": "Perubahan relatif suatu sifat fisik ketika suhu diubah sebesar satu derajat." },
  { "en": "Derating?", "id": "Operasi perangkat pada tingkat daya yang lebih rendah dari nilai maksimumnya untuk meningkatkan keandalan atau untuk beroperasi pada suhu lingkungan yang tinggi." },
  { "en": "Keandalan?", "id": "Kemungkinan suatu komponen atau sistem akan melakukan fungsi yang diperlukan tanpa kegagalan untuk periode waktu tertentu." },
  { "en": "Mean Time Between Failures (MTBF)?", "id": "Waktu rata-rata yang diprediksi berlalu antara kegagalan yang melekat pada sistem mekanis atau elektronik selama operasi normal." },
  { "en": "Analisis Modal dan Efek Kegagalan (FMEA)?", "id": "Pendekatan sistematis untuk mengidentifikasi potensi kegagalan dalam desain, proses, atau produk." },
  { "en": "Teknologi Hibrida?", "id": "Sirkuit elektronik yang dibangun dari kombinasi teknologi yang berbeda (misalnya, komponen diskrit dan sirkuit terpadu)." },
  { "en": "Monolithic Microwave Integrated Circuit (MMIC)?", "id": "Jenis sirkuit terpadu yang beroperasi pada frekuensi gelombang mikro." },
  { "en": "System in a Package (SiP)?", "id": "Sejumlah sirkuit terpadu yang tertutup dalam satu modul atau paket." },
  { "en": "Chiplet?", "id": "Die sirkuit terpadu kecil yang dirancang untuk digabungkan dengan chiplet lain dalam satu paket." },
  { "en": "Interposer?", "id": "Substrat listrik yang digunakan untuk merutekan koneksi antara satu soket atau koneksi ke yang lain." },
  { "en": "Through-Silicon Via (TSV)?", "id": "Koneksi listrik vertikal yang melewati wafer atau die silikon, digunakan dalam perakitan 3D IC." },
  { "en": "Wafer-Level Packaging (WLP)?", "id": "Proses mengemas sirkuit terpadu pada tingkat wafer, bukan setelah wafer dipotong menjadi die individu." },
  { "en": "Kapasitansi Parasit?", "id": "Kapasitansi yang tidak dapat dihindari dan biasanya tidak diinginkan yang ada antara bagian-bagian komponen atau sirkuit elektronik." },
  { "en": "Induktansi Parasit?", "id": "Induktansi yang tidak diinginkan dalam suatu sirkuit, sering kali berasal dari kabel komponen dan jejak PCB." },
  { "en": "Resistansi Parasit?", "id": "Resistansi yang tidak diinginkan dalam suatu sirkuit, seperti resistansi kabel atau jejak." },
  { "en": "Model SPICE?", "id": "Model teks yang menggambarkan perilaku komponen elektronik untuk digunakan dalam simulasi sirkuit." },
  { "en": "Analisis DC?", "id": "Jenis simulasi SPICE untuk menentukan titik operasi DC sirkuit." },
  { "en": "Analisis AC?", "id": "Jenis simulasi SPICE untuk menghitung respons frekuensi sinyal kecil dari suatu sirkuit." },
  { "en": "Analisis Transien?", "id": "Jenis simulasi SPICE untuk menghitung respons domain waktu dari suatu sirkuit." },
  { "en": "Analisis Monte Carlo?", "id": "Metode simulasi yang menggunakan pengambilan sampel acak berulang untuk mendapatkan hasil numerik, sering digunakan untuk menganalisis variasi komponen." },
  { "en": "Desain untuk Pengujian (DFT)?", "id": "Teknik desain yang menambahkan fitur pengujian ke desain perangkat keras untuk memudahkan pengujian." },
  { "en": "Boundary Scan?", "id": "Metode untuk menguji interkoneksi pada papan sirkuit cetak atau di dalam sirkuit terpadu." },
  { "en": "Joint Test Action Group (JTAG)?", "id": "Standar industri untuk verifikasi, pengujian, dan debugging perangkat." },
  { "en": "Built-In Self-Test (BIST)?", "id": "Mekanisme yang memungkinkan mesin untuk menguji dirinya sendiri." },
  { "en": "Cakupan Kesalahan (Fault Coverage)?", "id": "Persentase jenis kesalahan tertentu yang dapat dideteksi oleh satu set vektor uji." },
  { "en": "Model Kesalahan Stuck-at?", "id": "Model kesalahan yang digunakan dalam pengujian digital di mana sebuah sinyal diasumsikan macet pada logika 0 atau logika 1." },
  { "en": "Automatic Test Pattern Generation (ATPG)?", "id": "Proses otomatis untuk membuat vektor uji yang akan membedakan antara perilaku sirkuit yang benar dan yang salah." },
  { "en": "Akselerator Perangkat Keras?", "id": "Unit perangkat keras khusus yang dirancang untuk melakukan fungsi tertentu lebih efisien daripada perangkat lunak yang berjalan di CPU umum." },
  { "en": "Coprocessor?", "id": "Prosesor tambahan yang digunakan untuk melengkapi fungsi prosesor utama (CPU)." },
  { "en": "Floating-Point Unit (FPU)?", "id": "Bagian dari CPU yang secara khusus dirancang untuk melakukan operasi pada angka floating-point." },
  { "en": "End-of-Life (EOL)?", "id": "Tahap akhir dalam siklus hidup produk di mana produk tersebut tidak lagi diproduksi atau didukung." },
  { "en": "Kualifikasi?", "id": "Proses untuk menunjukkan bahwa suatu entitas memenuhi persyaratan yang ditentukan, seperti standar keandalan." },
  { "en": "Polarisasi Gelombang?", "id": "Orientasi osilasi medan listrik dari gelombang elektromagnetik." },
  { "en": "Polarisasi Linear?", "id": "Jenis polarisasi di mana vektor medan listrik terbatas pada satu bidang di sepanjang arah propagasi." },
  { "en": "Polarisasi Sirkular?", "id": "Jenis polarisasi di mana ujung vektor medan listrik menggambarkan lingkaran saat merambat." },
  { "en": "Polarisasi Eliptis?", "id": "Jenis polarisasi di mana ujung vektor medan listrik menggambarkan elips saat merambat." },
  { "en": "Angka Derau (Noise Figure)?", "id": "Simbol NF (satuan dB), ukuran penurunan rasio sinyal terhadap derau (SNR) yang disebabkan oleh komponen dalam rantai sinyal." },
  { "en": "Suhu Derau (Noise Temperature)?", "id": "Cara untuk mengekspresikan tingkat daya derau yang tersedia dari suatu komponen atau sumber." },
  { "en": "Titik Potong Orde Ketiga (IP3)?", "id": "Ukuran linearitas dari penguat atau mixer. Titik potong yang lebih tinggi menunjukkan linearitas yang lebih baik." },
  { "en": "Kompresi Penguatan (Gain Compression)?", "id": "Fenomena di mana penguatan perangkat berkurang saat daya sinyal masukan meningkat, menunjukkan perilaku nonlinier." },
  { "en": "Resonator Keramik?", "id": "Komponen elektronik yang menggunakan resonansi mekanis dari keramik piezoelektrik untuk menciptakan sinyal osilasi." },
  { "en": "Resonator SAW (Surface Acoustic Wave)?", "id": "Perangkat elektromekanis yang menggunakan gelombang akustik permukaan untuk mengimplementasikan fungsi seperti filter atau osilator." },
  { "en": "Resonator FBAR (Film Bulk Acoustic Resonator)?", "id": "Jenis resonator yang digunakan dalam filter dan osilator frekuensi tinggi, menawarkan faktor kualitas (Q) yang sangat tinggi." },
  { "en": "Power Amplifier (PA)?", "id": "Jenis penguat yang dirancang untuk meningkatkan besaran daya dari suatu sinyal, biasanya tahap akhir dalam rantai transmisi." },
  { "en": "Low-Noise Amplifier (LNA)?", "id": "Jenis penguat yang dirancang untuk menguatkan sinyal yang sangat lemah sambil meminimalkan penambahan derau, biasanya tahap pertama dalam rantai penerima." },
  { "en": "Mixer?", "id": "Sirkuit nonlinier yang menciptakan frekuensi baru dari dua sinyal yang diterapkan padanya (misalnya, untuk mengubah frekuensi)." },
  { "en": "Konversi Naik (Up-conversion)?", "id": "Proses pencampuran (mixing) untuk meningkatkan frekuensi pembawa suatu sinyal." },
  { "en": "Konversi Turun (Down-conversion)?", "id": "Proses pencampuran (mixing) untuk menurunkan frekuensi pembawa suatu sinyal." },
  { "en": "Penerima Superheterodyne?", "id": "Jenis penerima radio yang menggunakan pencampuran frekuensi untuk mengubah sinyal yang diterima menjadi frekuensi perantara (IF) yang tetap." },
  { "en": "Penerima Homodyne (Direct-Conversion)?", "id": "Jenis penerima di mana sinyal RF dikonversi langsung ke baseband (frekuensi nol) dalam satu tahap." },
  { "en": "Frekuensi Gambar (Image Frequency)?", "id": "Frekuensi masukan yang tidak diinginkan dalam penerima superheterodyne yang menghasilkan frekuensi perantara yang sama dengan sinyal yang diinginkan." },
  { "en": "Penolakan Gambar (Image Rejection)?", "id": "Ukuran kemampuan penerima untuk menolak sinyal pada frekuensi gambar." },
  { "en": "Local Oscillator (LO)?", "id": "Osilator elektronik yang digunakan dengan mixer untuk mengubah frekuensi sinyal." },
  { "en": "Automatic Gain Control (AGC)?", "id": "Sirkuit loop tertutup yang mengatur penguatan penguat untuk mempertahankan level sinyal keluaran yang konstan meskipun ada variasi pada sinyal masukan." },
  { "en": "Band-gap Reference?", "id": "Sirkuit referensi tegangan yang banyak digunakan dalam sirkuit terpadu, menghasilkan tegangan keluaran yang stabil terlepas dari variasi suhu." },
  { "en": "Sirkuit Startup?", "id": "Sirkuit yang memastikan sirkuit lain (seperti band-gap reference) mulai beroperasi dalam keadaan yang benar saat daya diterapkan." },
  { "en": "Soft Start?", "id": "Fitur dalam catu daya yang secara perlahan menaikkan tegangan keluaran untuk membatasi arus masuk (inrush current)." },
  { "en": "Arus Masuk (Inrush Current)?", "id": "Lonjakan arus masukan sesaat saat peralatan listrik pertama kali dinyalakan." },
  { "en": "Proteksi Arus Lebih (Overcurrent Protection)?", "id": "Fitur keamanan yang mematikan perangkat jika arus melebihi level yang aman." },
  { "en": "Proteksi Tegangan Lebih (Overvoltage Protection)?", "id": "Fitur keamanan yang memutus daya atau menjepit tegangan jika melebihi level yang aman." },
  { "en": "Proteksi Suhu Lebih (Thermal Shutdown)?", "id": "Fitur keamanan yang mematikan perangkat jika suhunya melebihi batas aman." },
  { "en": "Layout?", "id": "Desain fisik dari sirkuit terpadu atau papan sirkuit cetak." },
  { "en": "Physical Design Rule Checking (DRC)?", "id": "Proses memeriksa apakah tata letak (layout) sirkuit terpadu sesuai dengan serangkaian aturan desain geometris." },
  { "en": "Layout Versus Schematic (LVS)?", "id": "Proses memverifikasi bahwa tata letak fisik sirkuit terpadu sesuai dengan skema sirkuit aslinya." },
  { "en": "Ekstraksi Parasit?", "id": "Proses menghitung efek parasit (R, C, L) dari tata letak fisik untuk digunakan dalam simulasi yang lebih akurat." },
  { "en": "Timing Closure?", "id": "Proses berulang untuk memodifikasi desain sirkuit digital untuk memenuhi kendala waktu (timing constraints)." },
  { "en": "Standard Cell?", "id": "Unit logika dasar (seperti gerbang NAND) dengan tata letak yang telah dirancang sebelumnya, digunakan dalam desain sirkuit digital." },
  { "en": "Place and Route?", "id": "Tahap dalam desain sirkuit terpadu di mana sel-sel standar ditempatkan dan interkoneksi di antara mereka dibuat." },
  { "en": "Clock Tree Synthesis (CTS)?", "id": "Proses membangun jaringan distribusi clock dalam desain digital untuk meminimalkan skew dan delay." },
  { "en": "Antenna Effect (Plasma-Induced Gate Oxide Damage)?", "id": "Efek dalam fabrikasi IC di mana muatan terkumpul pada konduktor selama proses etsa, berpotensi merusak gerbang transistor." },
  { "en": "Scan Chain?", "id": "Teknik yang digunakan dalam DFT di mana flip-flop dihubungkan bersama dalam satu register geser panjang untuk memfasilitasi pengujian." },
  { "en": "Logic Equivalence Checking (LEC)?", "id": "Proses membandingkan dua representasi sirkuit (misalnya, RTL dan daftar gerbang) untuk memastikan fungsionalitasnya identik." },
  { "en": "Register Transfer Level (RTL)?", "id": "Abstraksi desain yang memodelkan sirkuit digital dengan aliran sinyal digital (data) antara register, dan operasi logika yang dilakukan pada sinyal tersebut." },
  { "en": "Behavioral Modeling?", "id": "Deskripsi tingkat tertinggi dari suatu sirkuit, hanya berfokus pada fungsionalitas tanpa memperhatikan implementasi perangkat keras." },
  { "en": "Structural Modeling?", "id": "Deskripsi sirkuit yang terdiri dari instansiasi komponen dan interkoneksinya." },
  { "en": "Testbench?", "id": "Lingkungan simulasi yang digunakan untuk memverifikasi kebenaran atau validitas suatu model desain." },
  { "en": "Assertion?", "id": "Pernyataan dalam kode verifikasi yang memeriksa apakah kondisi tertentu benar selama simulasi." },
  { "en": "Functional Coverage?", "id": "Metrik dalam verifikasi yang mengukur seberapa baik fitur fungsional dari suatu desain telah diuji." },
  { "en": "Code Coverage?", "id": "Metrik yang mengukur persentase baris kode sumber yang telah dieksekusi selama pengujian." },
  { "en": "Constrained Random Verification?", "id": "Metodologi verifikasi di mana stimulus acak dihasilkan dalam batasan yang ditentukan pengguna untuk menguji desain secara menyeluruh." },
  { "en": "Universal Verification Methodology (UVM)?", "id": "Metodologi standar untuk verifikasi sirkuit terpadu, terutama berdasarkan SystemVerilog." },
  { "en": "SystemVerilog?", "id": "Bahasa deskripsi dan verifikasi perangkat keras yang merupakan superset dari Verilog." },
  { "en": "AMBA (Advanced Microcontroller Bus Architecture)?", "id": "Standar bus on-chip yang banyak digunakan untuk menghubungkan dan mengelola blok fungsional dalam desain System-on-a-Chip (SoC)." },
  { "en": "AXI (Advanced eXtensible Interface)?", "id": "Protokol bus yang merupakan bagian dari standar AMBA, cocok untuk interkoneksi berkinerja tinggi." },
  { "en": "Wishbone?", "id": "Arsitektur bus komputer open-source untuk digunakan dalam desain sirkuit terpadu." },
  { "en": "Core?", "id": "Unit pemrosesan dalam sebuah CPU atau blok fungsional yang dapat digunakan kembali dalam desain SoC (IP Core)." },
  { "en": "Fabless Semiconductor Company?", "id": "Perusahaan yang merancang dan menjual perangkat keras dan chip semikonduktor tetapi tidak memproduksinya sendiri (outsourcing)." },
  { "en": "Foundry?", "id": "Pabrik fabrikasi semikonduktor yang membuat chip untuk perusahaan fabless." },
  { "en": "Node Proses (Process Node)?", "id": "Mengacu pada generasi spesifik dari teknologi fabrikasi semikonduktor (misalnya, 7 nm, 5 nm)." },
  { "en": "Yield?", "id": "Persentase die yang berfungsi dengan benar dari total die pada sebuah wafer." },
  { "en": "Reticle?", "id": "Piringan kuarsa atau kaca dengan pola sirkuit, digunakan sebagai photomask dalam proses fotolitografi." },
  { "en": "Stepper/Scanner?", "id": "Mesin yang digunakan dalam fotolitografi untuk mengekspos pola dari reticle ke wafer." },
  { "en": "Chemical-Mechanical Polishing (CMP)?", "id": "Proses menghaluskan permukaan wafer dengan menggunakan abrasi kimia dan mekanis." },
  { "en": "Annealing?", "id": "Proses perlakuan panas yang mengubah sifat material, digunakan dalam fabrikasi semikonduktor untuk mengaktifkan dopan dan memperbaiki kerusakan kristal." },
  { "en": "Implantasi Ion?", "id": "Teknik doping di mana ion pengotor dipercepat ke dalam substrat semikonduktor." },
  { "en": "Difusi?", "id": "Proses di mana dopan bergerak dari daerah konsentrasi tinggi ke konsentrasi rendah pada suhu tinggi." },
  { "en": "Epitaksi?", "id": "Proses menumbuhkan lapisan kristal tipis di atas permukaan kristal substrat." },
  { "en": "FinFET?", "id": "Jenis transistor multi-gerbang, di mana gerbang ditempatkan di sekitar 'sirip' (fin) semikonduktor, memungkinkan kontrol yang lebih baik." },
  { "en": "Gate-All-Around (GAA) FET?", "id": "Jenis transistor di mana gerbang mengelilingi saluran dari semua sisi, memberikan kontrol elektrostatik yang lebih baik daripada FinFET." },
  { "en": "High-k Dielectric?", "id": "Bahan dengan konstanta dielektrik tinggi yang digunakan sebagai isolator gerbang dalam transistor untuk mengurangi arus bocor." },
  { "en": "Metal Gate?", "id": "Penggunaan bahan logam sebagai elektroda gerbang dalam transistor CMOS, menggantikan polisilikon." },
  { "en": "Strained Silicon?", "id": "Teknik di mana silikon diregangkan atau ditekan untuk meningkatkan mobilitas pembawa muatan dan kinerja transistor." },
  { "en": "Silicon on Insulator (SOI)?", "id": "Teknologi fabrikasi di mana lapisan tipis silikon ditempatkan di atas lapisan isolator untuk mengurangi kapasitansi parasit." },
  { "en": "Sinyal Analog?", "id": "Sinyal kontinu yang merepresentasikan beberapa kuantitas yang bervariasi waktu." },
  { "en": "Sinyal Digital?", "id": "Sinyal yang menggunakan nilai-nilai diskrit (biasanya biner) untuk merepresentasikan informasi." },
  { "en": "Bit?", "id": "Unit informasi dasar dalam komputasi dan komunikasi digital, biasanya direpresentasikan sebagai 0 atau 1." },
  { "en": "Byte?", "id": "Unit informasi digital yang paling sering terdiri dari delapan bit." },
  { "en": "Word?", "id": "Unit data dengan ukuran tetap yang ditangani oleh set instruksi atau perangkat keras prosesor." },
  { "en": "Endianness?", "id": "Urutan di mana byte dari kata data multi-byte disimpan dalam memori (misalnya, Big-Endian, Little-Endian)." },
  { "en": "Floating Point?", "id": "Representasi angka yang memungkinkan rentang nilai yang sangat lebar dengan menggunakan notasi ilmiah." },
  { "en": "Fixed Point?", "id": "Representasi angka di mana titik biner (radix point) tetap pada posisi tertentu." },
  { "en": "Two's Complement?", "id": "Metode representasi matematika untuk bilangan biner bertanda." },
  { "en": "Gray Code?", "id": "Urutan sistem bilangan biner di mana dua nilai berturut-turut hanya berbeda satu bit." },
  { "en": "One-Hot Encoding?", "id": "Representasi di mana satu bit adalah 'hot' (1) dan semua bit lainnya adalah 'cold' (0)." },
  { "en": "Binary-Coded Decimal (BCD)?", "id": "Sistem pengkodean untuk angka desimal di mana setiap digit diwakili oleh urutan biner empat bitnya sendiri." },
  { "en": "ASCII (American Standard Code for Information Interchange)?", "id": "Standar pengkodean karakter untuk komunikasi elektronik." },
  { "en": "Unicode?", "id": "Standar pengkodean karakter internasional untuk berbagai bahasa dan skrip." },
  { "en": "Checksum?", "id": "Nilai kecil yang berasal dari blok data digital untuk tujuan mendeteksi kesalahan." },
  { "en": "Cyclic Redundancy Check (CRC)?", "id": "Kode pendeteksi kesalahan yang umum digunakan dalam jaringan digital dan perangkat penyimpanan." },
  { "en": "Firmware?", "id": "Perangkat lunak permanen yang diprogram ke dalam memori baca-saja." },
  { "en": "Bootloader?", "id": "Program komputer yang memuat sistem operasi utama saat komputer dinyalakan." },
  { "en": "Device Driver?", "id": "Program komputer yang mengoperasikan atau mengontrol jenis perangkat tertentu yang terpasang pada komputer." },
  { "en": "Operating System (OS)?", "id": "Perangkat lunak sistem yang mengelola perangkat keras dan sumber daya perangkat lunak komputer." },
  { "en": "Real-Time Operating System (RTOS)?", "id": "Sistem operasi yang dimaksudkan untuk melayani aplikasi real-time yang memproses data saat masuk, biasanya tanpa penundaan buffer." },
  { "en": "Interrupt Service Routine (ISR)?", "id": "Fungsi perangkat lunak yang dipanggil oleh prosesor sebagai respons terhadap interupsi." },
  { "en": "Stack?", "id": "Struktur data abstrak yang berfungsi sebagai kumpulan elemen, dengan dua operasi utama: push dan pop (LIFO)." },
  { "en": "Heap?", "id": "Area memori yang digunakan untuk alokasi memori dinamis." },
  { "en": "Pointer?", "id": "Objek dalam pemrograman yang menyimpan alamat memori." },
  { "en": "Memory Leak?", "id": "Terjadi ketika sebuah program secara keliru mengelola alokasi memori sehingga memori yang tidak lagi dibutuhkan tidak dilepaskan." },
  { "en": "Garbage Collection?", "id": "Bentuk manajemen memori otomatis di mana pengumpul sampah mencoba untuk merebut kembali memori yang ditempati oleh objek yang tidak lagi digunakan." },
  { "en": "Quantum Computing?", "id": "Penggunaan fenomena mekanika kuantum seperti superposisi dan keterkaitan untuk melakukan komputasi." },
  { "en": "Qubit (Quantum Bit)?", "id": "Unit dasar informasi kuantum, analog dengan bit klasik, tetapi dapat berada dalam superposisi 0 dan 1." },
  { "en": "Keterkaitan Kuantum (Quantum Entanglement)?", "id": "Fenomena fisika di mana keadaan kuantum dua atau lebih objek harus dijelaskan dengan mengacu satu sama lain, bahkan jika objek tersebut terpisah secara spasial." },
  { "en": "Superposisi Kuantum?", "id": "Prinsip dasar mekanika kuantum yang menyatakan bahwa partikel dapat berada di banyak keadaan pada saat yang bersamaan." },
  { "en": "Dekohorensi Kuantum?", "id": "Hilangnya koherensi kuantum, yaitu proses di mana sistem kuantum kehilangan sifat kuantumnya karena interaksi dengan lingkungan." },
  { "en": "Gerbang Logika Kuantum?", "id": "Sirkuit kuantum dasar yang beroperasi pada sejumlah kecil qubit, mirip dengan gerbang logika klasik." },
  { "en": "Komunikasi Kuantum?", "id": "Bidang fisika terapan dan teknik yang berfokus pada pengiriman informasi menggunakan sistem kuantum." },
  { "en": "Kriptografi Kuantum?", "id": "Penggunaan prinsip-prinsip mekanika kuantum untuk melakukan tugas-tugas kriptografi, menawarkan keamanan yang secara teoritis tidak dapat dipecahkan." },
  { "en": "Sensor Atom?", "id": "Sensor yang menggunakan sifat-sifat atom untuk melakukan pengukuran presisi tinggi terhadap kuantitas seperti waktu, percepatan, dan medan magnet." },
  { "en": "Jam Atom?", "id": "Jam yang menggunakan frekuensi resonansi atom sebagai osilatornya untuk pengukuran waktu yang sangat akurat." },
  { "en": "Magnetometer Atom?", "id": "Perangkat yang sangat sensitif untuk mengukur medan magnet, berdasarkan interaksi medan magnet dengan spin atom." },
  { "en": "Memristor?", "id": "Komponen listrik non-linier pasif dua terminal yang hambatannya bergantung pada riwayat arus yang telah melewatinya." },
  { "en": "Elektronik Organik?", "id": "Cabang ilmu material yang berurusan dengan desain, sintesis, dan penerapan molekul organik atau polimer yang menunjukkan sifat elektronik." },
  { "en": "OLED (Organic Light-Emitting Diode)?", "id": "Jenis LED di mana lapisan pemancar cahaya adalah film senyawa organik." },
  { "en": "OTFT (Organic Thin-Film Transistor)?", "id": "Transistor film tipis yang menggunakan semikonduktor organik dalam salurannya." },
  { "en": "Elektronik Fleksibel?", "id": "Teknologi untuk merakit sirkuit elektronik dengan memasang perangkat elektronik pada substrat plastik yang fleksibel." },
  { "en": "Elektronik Cetak?", "id": "Sekumpulan metode pencetakan yang digunakan untuk membuat perangkat listrik pada berbagai substrat." },
  { "en": "Tinta Konduktif?", "id": "Tinta yang mengandung partikel konduktif (seperti perak atau karbon) yang digunakan untuk mencetak sirkuit." },
  { "en": "Wearable Technology?", "id": "Kategori perangkat elektronik yang dapat dikenakan sebagai aksesori, ditanamkan di pakaian, atau bahkan ditanamkan di tubuh pengguna." },
  { "en": "Internet of Things (IoT)?", "id": "Jaringan objek fisik yang tertanam dengan sensor, perangkat lunak, dan teknologi lain untuk tujuan menghubungkan dan bertukar data melalui internet." },
  { "en": "Edge Computing?", "id": "Paradigma komputasi terdistribusi yang mendekatkan komputasi dan penyimpanan data ke sumber data, mengurangi latensi." },
  { "en": "Cloud Computing?", "id": "Penyediaan layanan komputasi—termasuk server, penyimpanan, basis data, jaringan, perangkat lunak—melalui Internet ('awan')." },
  { "en": "Power-On Self-Test (POST)?", "id": "Proses diagnostik yang dijalankan oleh firmware komputer saat pertama kali dihidupkan." },
  { "en": "BIOS (Basic Input/Output System)?", "id": "Firmware yang digunakan untuk melakukan inisialisasi perangkat keras selama proses booting." },
  { "en": "UEFI (Unified Extensible Firmware Interface)?", "id": "Spesifikasi yang mendefinisikan antarmuka perangkat lunak antara sistem operasi dan firmware platform, penerus BIOS." },
  { "en": "Trusted Platform Module (TPM)?", "id": "Standar internasional untuk mikrokontroler kriptografi yang aman, yang dapat menyimpan artefak kriptografi." },
  { "en": "Secure Boot?", "id": "Fitur keamanan yang dirancang untuk mencegah perangkat lunak berbahaya dimuat selama proses boot." },
  { "en": "Side-Channel Attack?", "id": "Serangan yang didasarkan pada informasi yang diperoleh dari implementasi fisik sistem komputer, bukan dari kelemahan dalam algoritma itu sendiri (misalnya, analisis daya)." },
  { "en": "Analisis Daya Diferensial (DPA)?", "id": "Jenis serangan side-channel yang menganalisis konsumsi daya perangkat kriptografi." },
  { "en": "Glitching?", "id": "Jenis serangan di mana gangguan (glitch) sengaja dimasukkan ke dalam sistem (misalnya, pada daya atau clock) untuk menyebabkan perilaku yang salah." },
  { "en": "Sirkuit Terpadu Fotonik (PIC)?", "id": "Chip yang berisi dua atau lebih komponen fotonik yang membentuk sirkuit fungsional." },
  { "en": "Sakelar Optik?", "id": "Komponen yang secara selektif mengalihkan sinyal optik dari satu jalur ke jalur lain." },
  { "en": "Modulator Optik?", "id": "Perangkat yang digunakan untuk memodulasi pancaran cahaya." },
  { "en": "Time-Domain Reflectometer (TDR)?", "id": "Instrumen yang digunakan untuk mengkarakterisasi dan menemukan kesalahan pada kabel logam dengan mengamati pantulan sinyal." },
  { "en": "Optical Time-Domain Reflectometer (OTDR)?", "id": "Instrumen optoelektronik yang digunakan untuk mengkarakterisasi serat optik." },
  { "en": "Bit-Error-Rate Tester (BERT)?", "id": "Peralatan pengujian elektronik yang digunakan untuk menguji kualitas transmisi sinyal dari satu titik ke titik lain." },
  { "en": "Protokol Analisator?", "id": "Alat yang digunakan untuk menangkap dan menganalisis sinyal dan lalu lintas data melalui saluran komunikasi." },
  { "en": "In-Circuit Emulator (ICE)?", "id": "Alat perangkat keras yang digunakan untuk men-debug perangkat lunak pada sistem tertanam." },
  { "en": "LCR Meter?", "id": "Jenis peralatan uji elektronik yang digunakan untuk mengukur induktansi (L), kapasitansi (C), dan resistansi (R) suatu komponen." },
  { "en": "Pengukur Impedansi?", "id": "Perangkat yang digunakan untuk mengukur impedansi kompleks." },
  { "en": "Penguji Transistor?", "id": "Instrumen untuk menguji perilaku listrik dari sebuah transistor." },
  { "en": "Penganalisis Audio?", "id": "Instrumen uji yang dirancang untuk mengukur kinerja peralatan audio." },
  { "en": "Beban Elektronik (Electronic Load)?", "id": "Instrumen yang mensimulasikan beban listrik untuk menguji catu daya dan sumber daya lainnya." },
  { "en": "Sumber Ukur (Source Measure Unit - SMU)?", "id": "Instrumen yang dapat secara bersamaan memberikan sumber tegangan atau arus dan mengukurnya." },
  { "en": "BNC Connector?", "id": "Jenis konektor RF kopling cepat miniatur yang digunakan untuk kabel koaksial." },
  { "en": "SMA Connector?", "id": "Konektor RF koaksial semi-presisi yang digunakan untuk aplikasi gelombang mikro." },
  { "en": "N-type Connector?", "id": "Konektor RF koaksial tahan cuaca berukuran sedang untuk penggunaan umum." },
  { "en": "DB-9/DB-25 Connector?", "id": "Jenis umum dari konektor D-subminiature, sering digunakan untuk koneksi serial seperti RS-232." },
  { "en": "RJ-45 Connector?", "id": "Konektor standar yang digunakan untuk jaringan Ethernet." },
  { "en": "Molex Connector?", "id": "Nama merek untuk berbagai konektor listrik, sering digunakan untuk daya di dalam komputer." },
  { "en": "Gage (Ukuran Kawat)?", "id": "Ukuran diameter kawat, seperti American Wire Gauge (AWG)." },
  { "en": "Skin Effect?", "id": "Kecenderungan arus bolak-balik (AC) untuk terdistribusi di dalam konduktor sehingga kepadatan arus lebih besar di dekat permukaan konduktor." },
  { "en": "Efek Kedekatan (Proximity Effect)?", "id": "Kecenderungan arus AC untuk mengalir dalam distribusi yang tidak merata di konduktor yang berdekatan." },
  { "en": "Litz Wire?", "id": "Jenis kabel yang terdiri dari banyak untaian kawat tipis yang terisolasi dan dipilin bersama, dirancang untuk mengurangi kerugian efek kulit dan kedekatan." },
  { "en": "Superkapasitor (Ultracapacitor)?", "id": "Kapasitor dengan nilai kapasitansi yang jauh lebih tinggi daripada kapasitor lainnya, tetapi dengan batas tegangan yang lebih rendah." },
  { "en": "Kapasitor Film?", "id": "Kapasitor yang menggunakan film plastik tipis sebagai dielektrik." },
  { "en": "Kapasitor Mika?", "id": "Kapasitor yang sangat stabil dan andal yang menggunakan mika sebagai dielektrik." },
  { "en": "Resistor Film Logam?", "id": "Jenis resistor aksial yang memiliki film logam resistif di sekitar inti keramik." },
  { "en": "Resistor Film Karbon?", "id": "Jenis resistor aksial umum yang menggunakan film karbon sebagai elemen resistif." },
  { "en": "Resistor Wirewound?", "id": "Resistor yang dibuat dengan melilitkan kawat logam di sekitar inti keramik, sering digunakan untuk peringkat daya tinggi." },
  { "en": "Jaringan Resistor?", "id": "Paket tunggal yang berisi beberapa resistor, seringkali dengan konfigurasi yang sama." },
  { "en": "Trimmer Kapasitor?", "id": "Kapasitor variabel yang dimaksudkan untuk penyesuaian awal dalam sirkuit dan jarang disesuaikan setelahnya." },
  { "en": "Galium Nitrida (GaN)?", "id": "Semikonduktor celah pita lebar yang sangat keras, digunakan dalam LED biru dan elektronika daya berkinerja tinggi." },
  { "en": "Silikon Karbida (SiC)?", "id": "Semikonduktor yang mengandung silikon dan karbon, digunakan dalam aplikasi daya dan suhu tinggi." },
  { "en": "Galium Arsenida (GaAs)?", "id": "Semikonduktor yang digunakan dalam pembuatan perangkat frekuensi tinggi dan sel surya." },
  { "en": "Indium Fosfida (InP)?", "id": "Semikonduktor yang digunakan dalam elektronika frekuensi tinggi dan perangkat fotonik." },
  { "en": "Substrat?", "id": "Bahan dasar di mana fabrikasi perangkat semikonduktor terjadi." },
  { "en": "Wafer Boat?", "id": "Pembawa yang digunakan untuk menahan beberapa wafer semikonduktor selama langkah-langkah pemrosesan." },
  { "en": "Cleanroom?", "id": "Lingkungan terkontrol dengan tingkat polutan yang sangat rendah, penting untuk fabrikasi semikonduktor." },
  { "en": "Kelas Cleanroom (misalnya, Kelas 100)?", "id": "Klasifikasi yang menunjukkan jumlah partikel per kaki kubik udara." },
  { "en": "Dicing?", "id": "Proses memotong wafer semikonduktor menjadi die-die individual." },
  { "en": "Pick-and-Place Machine?", "id": "Mesin robotik yang digunakan untuk menempatkan komponen pemasangan permukaan ke papan sirkuit cetak." },
  { "en": "CAD (Computer-Aided Design)?", "id": "Penggunaan komputer untuk membantu dalam pembuatan, modifikasi, analisis, atau optimisasi desain." },
  { "en": "CAM (Computer-Aided Manufacturing)?", "id": "Penggunaan perangkat lunak untuk mengontrol peralatan mesin dan mesin terkait dalam pembuatan komponen." },
  { "en": "Gerber File?", "id": "Format file standar yang digunakan oleh industri PCB untuk menggambarkan gambar papan sirkuit." },
  { "en": "Bill of Materials (BOM)?", "id": "Daftar bahan baku, sub-rakitan, komponen, dan kuantitas dari masing-masing yang dibutuhkan untuk memproduksi suatu produk." },
  { "en": "First-Pass Yield?", "id": "Persentase produk yang diproduksi dengan benar dan sesuai spesifikasi tanpa perlu pengerjaan ulang." },
  { "en": "Pengerjaan Ulang (Rework)?", "id": "Operasi perbaikan pada produk atau komponen yang tidak sesuai." },
  { "en": "Siklus Hidup Produk?", "id": "Proses yang dilalui produk dari pengenalan hingga penurunan atau penarikan." },
  { "en": "Obsolescence?", "id": "Keadaan menjadi usang atau tidak lagi diproduksi atau digunakan." },
  { "en": "RoHS (Restriction of Hazardous Substances)?", "id": "Arahan yang membatasi penggunaan bahan berbahaya tertentu dalam peralatan listrik dan elektronik." },
  { "en": "WEEE (Waste Electrical and Electronic Equipment)?", "id": "Arahan yang menetapkan target pengumpulan, daur ulang, dan pemulihan untuk semua jenis barang listrik." },
  { "en": "CE Marking?", "id": "Tanda sertifikasi yang menunjukkan kesesuaian dengan standar kesehatan, keselamatan, dan perlindungan lingkungan untuk produk yang dijual di dalam Area Ekonomi Eropa." },
  { "en": "FCC (Federal Communications Commission) Certification?", "id": "Sertifikasi untuk perangkat elektronik yang diproduksi atau dijual di Amerika Serikat yang menyatakan bahwa interferensi elektromagnetiknya berada di bawah batas yang disetujui." },
  { "en": "UL (Underwriters Laboratories) Certification?", "id": "Sertifikasi yang menunjukkan bahwa suatu produk telah diuji untuk standar keamanan yang diakui secara nasional." },
  { "en": "IP Rating (Ingress Protection)?", "id": "Peringkat yang mengklasifikasikan tingkat perlindungan yang diberikan terhadap intrusi benda padat dan air." },
  { "en": "NEMA Enclosure Ratings?", "id": "Standar yang menentukan jenis-jenis selungkup listrik berdasarkan tingkat perlindungan yang mereka berikan." },
  { "en": "Solenoid Driver?", "id": "Sirkuit yang dirancang untuk menyediakan arus yang cukup untuk menggerakkan solenoida." },
  { "en": "H-Bridge?", "id": "Sirkuit elektronik yang memungkinkan tegangan diterapkan di seluruh beban dalam kedua arah, umum digunakan untuk menggerakkan motor DC." },
  { "en": "Motor Stepper?", "id": "Motor DC tanpa sikat yang membagi putaran penuh menjadi sejumlah langkah yang sama." },
  { "en": "Motor Servo?", "id": "Aktuator putar atau linier yang memungkinkan kontrol presisi terhadap posisi sudut atau linier." },
  { "en": "Back-EMF (Back Electromotive Force)?", "id": "Tegangan yang berlawanan dengan arus yang menginduksinya, terjadi pada motor dan induktor." },
  { "en": "Peltier Cooler/Heater?", "id": "Perangkat termoelektrik yang mentransfer panas dari satu sisi ke sisi lain saat arus melewatinya." },
  { "en": "Heaviside Step Function?", "id": "Fungsi diskontinu yang nilainya nol untuk argumen negatif dan satu untuk argumen positif." },
  { "en": "Dirac Delta Function?", "id": "Fungsi umum yang memiliki nilai tak terhingga pada x=0 dan nol di tempat lain, dengan integral total satu." },
  { "en": "Konstanta Dielektrik?", "id": "Simbol κ, ukuran kemampuan suatu zat untuk menyimpan energi listrik dalam medan listrik." }

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
