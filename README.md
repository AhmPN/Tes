 <!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kirim Pesan Cinta 💌</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #ffe6e6;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }
        .card {
            background: white;
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
            width: 350px;
        }
        h2 {
            color: #d9534f;
            text-align: center;
        }
        label {
            font-weight: bold;
            color: #333;
        }
        input, textarea {
            width: 100%;
            padding: 10px;
            margin-top: 5px;
            margin-bottom: 15px;
            border: 1px solid #ccc;
            border-radius: 5px;
            box-sizing: border-box;
        }
        button {
            width: 100%;
            background-color: #d9534f;
            color: white;
            border: none;
            padding: 12px;
            border-radius: 5px;
            font-size: 16px;
            cursor: pointer;
        }
        button:hover {
            background-color: #c9302c;
        }
    </style>
</head>
<body>
    <div class="card">
        <h2>Pesan untuk Pacar ❤️</h2>
        <!-- Ganti email_pacar@gmail.com dengan email pacar Anda -->
        <form action="mailto:ahmadputra.nurrohim@gmail.com" method="GET" enctype="text/plain">       
            <label for="subject">Subjek Pesan:</label>
            <input type="text" id="subject" name="subject" value="Pesan Spesial untuk Kamu ❤️" required>
            <label for="body">Isi Pesan Cinta:</label>
            <textarea id="body" name="body" rows="5" placeholder="Tulis ungkapan hatimu di sini..." required></textarea>
            <button type="submit">Buka & Kirim Pesan</button>
        </form>
    </div>
</body>
</html>
