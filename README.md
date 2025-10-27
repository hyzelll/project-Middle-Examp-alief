# project-Middle-Examp-alief

// Data Input
const lahan = [
    ["subur", "kering", "subur", "subur"],
    ["tandus", "kering", "kering", "subur"],
    ["subur", "subur", "subur", "kering"],
    ["kering", "kering", "kering", "kering"]
];

const weatherData = {
    temperature: 28, // 20 <= T <= 30 (OK)
    humidity: 55,    // > 50 (OK)
    windSpeed: 12    // < 15 (OK)
};

/**
 * Memeriksa apakah kondisi cuaca ideal untuk bercocok tanam.
 */
function isWeatherIdeal(weather) {
    const isTempIdeal = weather.temperature >= 20 && weather.temperature <= 30;
    const isHumidityIdeal = weather.humidity > 50;
    const isWindIdeal = weather.windSpeed < 15;
    return isTempIdeal && isHumidityIdeal && isWindIdeal;
}

/**
 * Fungsi utama untuk memproses lahan dan cuaca.
 */
function prosesManajemenLahan(lahanMatrix, cuaca) {
    let totalPetakSuburSetelahProses = 0;
    let totalPetakDitanami = 0;
    
    // Duplikat lahan agar tidak mengubah array asli
    const lahanYangDiproses = JSON.parse(JSON.stringify(lahanMatrix));

    // A. PEMROSESAN STATUS LAHAN (Rule 50% Subur)
    for (let i = 0; i < lahanYangDiproses.length; i++) {
        const baris = lahanYangDiproses[i];
        let suburCount = 0;
        const totalPetak = baris.length;
        const minimumSubur = totalPetak * 0.5;

        // Hitung petak subur di baris
        for (let j = 0; j < totalPetak; j++) {
            if (baris[j] === "subur") {
                suburCount++;
            }
        }

        // Terapkan aturan 50%
        if (suburCount < minimumSubur) {
            // Semua petak yang BUKAN "tandus" menjadi "kering"
            for (let k = 0; k < totalPetak; k++) {
                if (baris[k] !== "tandus") {
                    lahanYangDiproses[i][k] = "kering";
                }
            }
        }
    }

    // B. HITUNG TOTAL PETAK SUBUR SETELAH PROSES
    for (let i = 0; i < lahanYangDiproses.length; i++) {
        for (let j = 0; j < lahanYangDiproses[i].length; j++) {
            if (lahanYangDiproses[i][j] === "subur") {
                totalPetakSuburSetelahProses++;
            }
        }
    }

    // C. PENGECEKAN CUACA DAN PERHITUNGAN PETAK DITANAMI
    const cuacaIdeal = isWeatherIdeal(cuaca);

    console.log("===== SOAL 1: MANAJEMEN LAHAN PERKEBUNAN =====");
    console.log("Total petak subur: " + totalPetakSuburSetelahProses);

    if (cuacaIdeal) {
        totalPetakDitanami = totalPetakSuburSetelahProses;
        console.log("Total petak yang ditanami: " + totalPetakDitanami);
        console.log("Cuaca cocok untuk bercocok tanam, sehingga tidak ada peringatan.");
    } else {
        totalPetakDitanami = 0;
        console.log("Total petak yang ditanami: " + totalPetakDitanami);
        console.log("PERINGATAN: Cuaca tidak cocok untuk bercocok tanam.");
    }
}

// Jalankan Program
prosesManajemenLahan(lahan, weatherData);
