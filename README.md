L<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Animasi Jantung Partikel Biru</title>
    <style>
        body, html {
            margin: 0;
            padding: 0;
            width: 100%;
            height: 100%;
            overflow: hidden;
            background-color: black;
            display: flex;
            justify-content: center;
            align-items: center;
        }
        #canvas {
            display: block;
        }
        /* Gaya untuk teks opsional (seperti dalam gambar) */
        .bottom-ui {
            position: absolute;
            bottom: 10px;
            left: 10px;
            font-family: monospace;
            color: rgba(0, 150, 255, 0.5);
            font-size: 10px;
            pointer-events: none;
        }
    </style>
</head>
<body>
<canvas id="canvas"></canvas>
<!-- Elemen UI opsional untuk meniru strip bawah di gambar -->
<div class="bottom-ui">
    -- [DATA SCAN: HEART_CORE_01] --<br>
    STATUS: ACTIVE<br>
    POWER: STABLE<br>
    LOCATION: VOID_QUADRANT
</div>
<script>
    // 1. Setup Canvas
    const canvas = document.getElementById('canvas');
    const ctx = canvas.getContext('2d');
    let width, height;
    function resize() {
        width = canvas.width = window.innerWidth;
        height = canvas.height = window.innerHeight;
    }
    window.addEventListener('resize', resize);
    resize();
    // 2. Fungsi untuk membuat bentuk jantung (matematika)
    // Parameter 't' adalah sudut dari 0 ke 2*pi
    function getHeartPoint(t, size) {
        // Rumus x, y untuk jantung parametrik
        const x = 16 * Math.pow(Math.sin(t), 3);
        const y = -(13 * Math.cos(t) - 5 * Math.cos(2 * t) - 2 * Math.cos(3 * t) - Math.cos(4 * t));
        return { x: x * size, y: y * size };
    }
    // 3. Kelas Partikel
    class Particle {
        constructor(isCore, targetPos = null) {
            this.isCore = isCore; // Apakah partikel membentuk jantung atau hanyut bebas
            this.targetPos = targetPos; // Posisi target pada jantung       
            // Posisi awal acak
            this.x = Math.random() * width;
            this.y = Math.random() * height;       
            // Kecepatan
            this.vx = (Math.random() - 0.5) * 1.5;
            this.vy = (Math.random() - 0.5) * 1.5;            
            // Ukuran dan warna
            this.size = Math.random() * 2 + 0.5;           
            // Hue biru neon
            const hue = 210 + Math.random() * 20; 
            const sat = 100;
            const light = 60 + Math.random() * 30;
            this.color = `hsla(${hue}, ${sat}%, ${light}%, ${Math.random() * 0.8 + 0.2})`;           
            // Properti untuk gerakan mengapung
            this.floatPhase = Math.random() * Math.PI * 2;
            this.floatSpeed = Math.random() * 0.05 + 0.02;
            this.floatAmp = Math.random() * 5 + 2;
            // Untuk partikel hanyut
            this.alpha = 1;
            this.decay = Math.random() * 0.005 + 0.002;
        }
        update(time) {
            if (this.isCore && this.targetPos) {
                // Perhitungan gerakan mengapung (floating/breathing)
                const floatY = Math.sin(time * this.floatSpeed + this.floatPhase) * this.floatAmp;           
                // Pindahkan menuju target di jantung
                const dx = (this.targetPos.x + width / 2) - this.x;
                const dy = (this.targetPos.y + height / 2 + floatY) - this.y;             
                // Gunakan akselerasi kecil untuk "menarik" ke target
                this.vx += dx * 0.01;
                this.vy += dy * 0.01;              
                // Tambahkan hambatan agar tidak bergetar terlalu cepat
                this.vx *= 0.94;
                this.vy *= 0.94;
                this.x += this.vx;
                this.y += this.vy;
            } else {
                // Gerakan untuk partikel hanyut bebas (bukan jantung)
                this.x += this.vx;
                this.y += this.vy;
                this.alpha -= this.decay;
                // Bungkus di sekitar layar
                if (this.x < 0) this.x = width;
                if (this.x > width) this.x = 0;
                if (this.y < 0) this.y = height;
                if (this.y > height) this.y = 0;
            }
        }
        draw() {
            ctx.fillStyle = this.color;
            if (!this.isCore) ctx.globalAlpha = Math.max(0, this.alpha);            
            ctx.beginPath();
            ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
            ctx.fill();            
            ctx.globalAlpha = 1; // Reset alpha
        }
    }
    // 4. Inisialisasi Partikel
    const coreParticles = [];
    const driftParticles = [];
    const numCoreParticles = 1500; // Jumlah partikel yang membentuk jantung
    const numDriftParticles = 200; // Partikel latar belakang
    const heartSizeScale = Math.min(width, height) / 50; // Ukuran jantung proporsional dengan layar
    // Buat partikel jantung
    for (let i = 0; i < numCoreParticles; i++) {
        const t = (i / numCoreParticles) * Math.PI * 2;
        const targetPos = getHeartPoint(t, heartSizeScale);
        coreParticles.push(new Particle(true, targetPos));
    }
    // Buat partikel hanyut
    function createDriftParticle() {
        driftParticles.push(new Particle(false));
    }
    for (let i = 0; i < numDriftParticles; i++) {
        createDriftParticle();
    }
    // 5. Loop Animasi
    let startTime = Date.now();
    function animate() {
        requestAnimationFrame(animate);        
        // Hapus kanvas dengan 'trail effect' (lapisan semi-transparan)
        ctx.fillStyle = 'rgba(0, 0, 0, 0.15)'; // Nilai 0.15 memberikan trail yang bagus
        ctx.fillRect(0, 0, width, height);        
        const currentTime = (Date.now() - startTime) * 0.001; // Waktu dalam detik        
        // Buat partikel jantung memiliki kilau (glow)
        ctx.shadowBlur = 10;
        ctx.shadowColor = 'rgba(0, 200, 255, 0.8)';        
        // Perbarui dan gambar partikel jantung
        coreParticles.forEach(p => {
            p.update(currentTime);
            p.draw();
        });        
        // Matikan bayangan untuk partikel latar belakang agar lebih efisien
        ctx.shadowBlur = 0;        
        // Perbarui dan gambar partikel hanyut
        for (let i = driftParticles.length - 1; i >= 0; i--) {
            const p = driftParticles[i];
            p.update(currentTime);
            p.draw();
            // Buat partikel baru jika sudah pudar
            if (p.alpha <= 0) {
                driftParticles.splice(i, 1);
                createDriftParticle();
            }
        }
    }
    // 6. Mulai
    animate();
</script>
</body>
</html>
