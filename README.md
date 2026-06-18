
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dashboard Ubinan Komoditas Padi Sawah </title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        :root {
            --bg-color: #f4f6f9;
            --card-bg: #ffffff;
            --text-main: #2c3e50;
            --text-sub: #666666;
            --primary: #3498db;
            --success: #2ecc71;
            --warning: #f39c12;
            --shadow: 0 4px 12px rgba(0,0,0,0.05);
            --radius: 12px;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: 'Segoe UI', Arial, sans-serif;
            background-color: var(--bg-color);
            color: var(--text-main);
            padding: 20px;
            line-height: 1.6;
        }

        .header {
            text-align: center;
            margin-bottom: 30px;
            padding: 10px;
        }

        .header h1 {
            font-size: 1.8rem;
            margin-bottom: 8px;
            color: var(--text-main);
        }

        .subtitle {
            color: var(--text-sub);
            font-size: 1rem;
        }

        /* KPI Container - Responsif */
        .kpi-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }

        .kpi {
            background: var(--card-bg);
            padding: 20px;
            border-radius: var(--radius);
            box-shadow: var(--shadow);
            text-align: center;
            transition: transform 0.2s;
        }

        .kpi:hover {
            transform: translateY(-3px);
        }

        .kpi h2 {
            font-size: 1.8rem;
            color: var(--text-main);
            margin-bottom: 5px;
        }

        .kpi p {
            color: var(--text-sub);
            font-size: 0.9rem;
            font-weight: 500;
        }

        /* Dashboard Layout Grid */
        .dashboard-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 25px;
            margin-bottom: 25px;
        }

        .chart-box {
            background: var(--card-bg);
            padding: 20px;
            border-radius: var(--radius);
            box-shadow: var(--shadow);
            position: relative;
            min-height: 320px;
        }

        .chart-full {
            grid-column: span 2;
            background: var(--card-bg);
            padding: 20px;
            border-radius: var(--radius);
            box-shadow: var(--shadow);
            position: relative;
            min-height: 350px;
            margin-bottom: 25px;
        }

        .chart-box h3, .chart-full h3, .insight-box h3 {
            font-size: 1.1rem;
            margin-bottom: 15px;
            color: var(--text-main);
            border-left: 4px solid var(--primary);
            padding-left: 10px;
        }

        /* Wrapper canvas agar Chart.js responsif */
        .chart-wrapper {
            position: relative;
            width: 100%;
            height: 250px;
        }

        .chart-full .chart-wrapper {
            height: 300px;
        }

        .note {
            margin-top: 15px;
            font-size: 0.85rem;
            color: #7f8c8d;
            font-style: italic;
        }

        .insight-box {
            background: var(--card-bg);
            padding: 25px;
            border-radius: var(--radius);
            box-shadow: var(--shadow);
            margin-top: 30px;
        }

        .insight-box ul {
            list-style-position: inside;
            padding-left: 5px;
        }

        .insight-box li {
            margin-bottom: 10px;
            font-size: 0.95rem;
        }

        /* MEDIA QUERIES (Kunci agar tidak berantakan di HP/Tablet) */
        @html-media-queries { }
        @media (max-width: 992px) {
            .dashboard-grid {
                grid-template-columns: 1fr;
            }
            .chart-full {
                grid-column: span 1;
            }
        }

        @media (max-width: 480px) {
            body {
                padding: 10px;
            }
            .header h1 {
                font-size: 1.4rem;
            }
            .kpi h2 {
                font-size: 1.5rem;
            }
        }
    </style>
</head>
<body>

<div class="header">
    <h1>Dashboard Ubinan Komoditas Padi Sawah</h1>
    <p class="subtitle">Perbandingan Produktivitas, Hasil Panen, dan Luas Lahan Berdasarkan Subground</p>
</div>

<div class="kpi-container">
    <div class="kpi">
        <h2>1.116,10</h2>
        <p>Produktivitas Rata-rata</p>
    </div>
    <div class="kpi">
        <h2>1.728,70 kg</h2>
        <p>Total Hasil Panen</p>
    </div>
    <div class="kpi">
        <h2>1.122.869 m²</h2>
        <p>Total Luas Lahan</p>
    </div>
    <div class="kpi">
        <h2>21</h2>
        <p>Jumlah Kecamatan</p>
    </div>
</div>

<div class="dashboard-grid">
    <div class="chart-box">
        <h3>Produktivitas Ubinan per Subground</h3>
        <div class="chart-wrapper"><canvas id="produktivitas"></canvas></div>
    </div>

    <div class="chart-box">
        <h3>Total Hasil Panen per Subground</h3>
        <div class="chart-wrapper"><canvas id="hasil"></canvas></div>
    </div>

    <div class="chart-box">
        <h3>Total Luas Lahan Sampel per Subground</h3>
        <div class="chart-wrapper"><canvas id="luas"></canvas></div>
        <div class="note">Catatan: Luas lahan sampel ubinan menggunakan ukuran <b>6,25 m²</b>.</div>
    </div>

    <div class="chart-box">
        <h3>Kontribusi Hasil Panen per Subground</h3>
        <div class="chart-wrapper"><canvas id="pieHasil"></canvas></div>
    </div>
</div>

<div class="chart-full">
    <h3>Perbandingan Hasil Ubinan per Kecamatan Berdasarkan Subground</h3>
    <div class="chart-wrapper"><canvas id="kecamatan"></canvas></div>
</div>

<div class="chart-full">
    <h3>Perbandingan Luas Lahan Sampel per Kecamatan Berdasarkan Subground</h3>
    <div class="chart-wrapper"><canvas id="luasKecamatan"></canvas></div>
    <div class="note">NB: Luas sampel ubinan setiap plot adalah <b>6,25 m²</b>.</div>
</div>

<div class="chart-full">
    <h3>Perbandingan Produktivitas Ubinan per Kecamatan Berdasarkan Subground</h3>
    <div class="chart-wrapper"><canvas id="produktKecamatan"></canvas></div>
</div>

<div class="chart-full">
    <h3>Top 10 Kecamatan Berdasarkan Total Hasil Ubinan</h3>
    <div class="chart-wrapper"><canvas id="topKecamatan"></canvas></div>
</div>

<div class="insight-box">
    <h3>Insight Data Ubinan</h3>
    <ul id="insightList"></ul>
</div>

<script>
// --- DATA SELECTION ---
const triwulan = ["Subground 1", "Subground 2", "Subground 3"];
const produktivitas = [1382.94, 1146.95, 818.42];
const hasil = [790.53, 580.46, 357.71];
const luas = [450932, 397612, 274325];

const kecamatan = [
    "Bandar Kedungmulyo", "Perak", "Gudo", "Diwek", "Ngoro", "Mojowarno",
    "Bareng", "Wonosalam", "Mojoagung", "Sumobito", "Jogoroto", "Peterongan",
    "Jombang", "Megaluh", "Tembelang", "Kesamben", "Kudu", "Ngusikan",
    "Ploso", "Kabuh", "Plandaan"
];

const triwulan1 = [14.12, 40.29, 65.38, 39.98, 15.40, 129.14, 35.74, 0.00, 79.09, 16.01, 25.23, 0.00, 12.96, 0.00, 0.00, 11.05, 66.11, 80.70, 16.33, 37.57, 105.45];
const triwulan2 = [42.09, 11.56, 26.98, 15.76, 15.43, 102.18, 47.52, 16.95, 31.22, 71.58, 11.25, 34.95, 34.17, 0.00, 54.77, 17.71, 6.68, 10.57, 5.53, 0.00, 23.61];
const triwulan3 = [10.62, 9.08, 13.24, 0.00, 8.79, 64.43, 19.94, 15.19, 20.13, 14.98, 0.00, 0.00, 36.06, 56.01, 35.02, 35.10, 4.03, 0.00, 10.56, 0.00, 4.55];

const luas_tw1 = [4550, 3700, 68350, 40750, 4940, 67600, 19600, 0, 54600, 7700, 20582, 0, 37800, 0, 0, 7000, 21770, 30460, 4200, 19330, 38000];
const luas_tw2 = [24120, 2430, 8615, 9380, 12685, 84000, 24680, 9500, 21700, 41059, 8840, 19820, 56400, 0, 50890, 7980, 2240, 5600, 2100, 0, 5573];
const luas_tw3 = [3380, 1830, 3100, 0, 3455, 42000, 10230, 16100, 9800, 22200, 0, 0, 69300, 25130, 23800, 37700, 2100, 0, 2800, 0, 1400];

const prod_tw1 = [10.68, 10.35, 217.58, 127.22, 15.31, 212.40, 50.11, 0.00, 191.07, 25.84, 49.04, 0.00, 102.06, 0.00, 0.00, 23.98, 76.24, 104.84, 14.29, 58.16, 93.80];
const prod_tw2 = [66.71, 5.75, 24.37, 32.82, 31.09, 275.00, 56.64, 25.21, 69.75, 124.15, 16.79, 52.87, 154.07, 0.00, 144.96, 22.08, 4.71, 18.64, 7.26, 0.00, 14.07];
const prod_tw3 = [11.38, 5.15, 8.62, 0.00, 9.53, 130.89, 31.56, 38.25, 31.31, 69.28, 0.00, 0.00, 231.02, 63.56, 66.39, 102.98, 5.28, 0.00, 9.24, 0.00, 3.98];

// --- OPTIONS UTILS ---
const commonOptions = {
    responsive: true,
    maintainAspectRatio: false,
    plugins: {
        legend: { position: 'bottom' }
    }
};

// --- RENDER CHARTS ---
new Chart(document.getElementById("produktivitas"), {
    type: 'bar',
    data: {
        labels: triwulan,
        datasets: [{ label: "Produktivitas", data: produktivitas, backgroundColor: '#3498db' }]
    },
    options: commonOptions
});

new Chart(document.getElementById("hasil"), {
    type: 'line',
    data: {
        labels: triwulan,
        datasets: [{ label: "Total Hasil Panen", data: hasil, borderColor: '#2ecc71', backgroundColor: 'rgba(46, 204, 113, 0.1)', fill: true, tension: 0.3 }]
    },
    options: commonOptions
});

new Chart(document.getElementById("luas"), {
    type: 'bar',
    data: {
        labels: triwulan,
        datasets: [{ label: "Total Luas Lahan", data: luas, backgroundColor: '#e67e22' }]
    },
    options: commonOptions
});

new Chart(document.getElementById("pieHasil"), {
    type: 'pie',
    data: {
        labels: triwulan,
        datasets: [{ data: hasil, backgroundColor: ["#3498db", "#2ecc71", "#f39c12"] }]
    },
    options: commonOptions
});

new Chart(document.getElementById("kecamatan"), {
    type: 'bar',
    data: {
        labels: kecamatan,
        datasets: [
            { label: "Subground 1", data: triwulan1, backgroundColor: '#3498db' },
            { label: "Subground 2", data: triwulan2, backgroundColor: '#2ecc71' },
            { label: "Subground 3", data: triwulan3, backgroundColor: '#f39c12' }
        ]
    },
    options: commonOptions
});

new Chart(document.getElementById("luasKecamatan"), {
    type: 'bar',
    data: {
        labels: kecamatan,
        datasets: [
            { label: "Subground 1", data: luas_tw1, backgroundColor: '#34495e' },
            { label: "Subground 2", data: luas_tw2, backgroundColor: '#9b59b6' },
            { label: "Subground 3", data: luas_tw3, backgroundColor: '#1abc9c' }
        ]
    },
    options: commonOptions
});

new Chart(document.getElementById("produktKecamatan"), {
    type: 'bar',
    data: {
        labels: kecamatan,
        datasets: [
            { label: "Subground 1", data: prod_tw1, backgroundColor: '#16a085' },
            { label: "Subground 2", data: prod_tw2, backgroundColor: '#d35400' },
            { label: "Subground 3", data: prod_tw3, backgroundColor: '#7f8c8d' }
        ]
    },
    options: commonOptions
});

let totalHasil = kecamatan.map((k, i) => ({
    nama: k,
    total: triwulan1[i] + triwulan2[i] + triwulan3[i]
}));

let top10 = [...totalHasil].sort((a, b) => b.total - a.total).slice(0, 10);

new Chart(document.getElementById("topKecamatan"), {
    type: 'bar',
    data: {
        labels: top10.map(x => x.nama),
        datasets: [{
            label: "Total Hasil Ubinan",
            data: top10.map(x => x.total),
            backgroundColor: "#e74c3c"
        }]
    },
    options: commonOptions
});

// --- INSIGHT LOGIC ---
let insight = document.getElementById("insightList");
let maxHasil = totalHasil.reduce((a, b) => a.total > b.total ? a : b);
let totalProd = kecamatan.map((k, i) => ({
    nama: k,
    total: prod_tw1[i] + prod_tw2[i] + prod_tw3[i]
}));
let maxProd = totalProd.reduce((a, b) => a.total > b.total ? a : b);
let triwulanMax = hasil.indexOf(Math.max(...hasil));

insight.innerHTML += `<li>Kecamatan dengan total hasil ubinan padi sawah tertinggi adalah <b>${maxHasil.nama}</b>.</li>`;
insight.innerHTML += `<li>Kecamatan dengan total produktivitas ubinan tertinggi adalah <b>${maxProd.nama}</b>.</li>`;
insight.innerHTML += `<li>Hasil panen tertinggi terjadi pada <b>Subground ${triwulanMax + 1}</b>.</li>`;
insight.innerHTML += `<li>Dashboard ini menganalisis data ubinan padi sawah dari <b>21 kecamatan</b> berdasarkan periode Subground.</li>`;
</script>

</body>
</html>
