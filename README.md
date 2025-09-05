<!DOCTYPE html>
<html lang="hy">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>QR Կոդ Գեներատոր</title>
  <style>
    body {
      font-family: "Segoe UI", sans-serif;
      background: #f0f4f8;
      margin: 0;
      padding: 20px;
    }

    h1 {
      text-align: center;
      color: #2c3e50;
      font-size: 36px;
      margin-bottom: 40px;
      text-shadow: 2px 2px 5px rgba(0,0,0,0.2);
    }

    .container {
      display: flex;
      justify-content: center;
      gap: 40px;
      flex-wrap: wrap;
      margin-bottom: 40px;
    }

    .section {
      border-radius: 16px;
      box-shadow: 0 6px 18px rgba(0,0,0,0.15);
      padding: 25px;
      width: 320px;
      transition: transform 0.3s, box-shadow 0.3s;
    }

    /* Գունային տարբերություններ բաժինների համար */
    .section:nth-child(1) {
      background: linear-gradient(135deg, #74b9ff, #0984e3);
      color: white;
    }
    .section:nth-child(2) {
      background: linear-gradient(135deg, #55efc4, #00b894);
      color: white;
    }
    .section:nth-child(3) {
      background: linear-gradient(135deg, #ffeaa7, #fdcb6e);
      color: #2c3e50;
    }

    .section:hover {
      transform: translateY(-5px);
      box-shadow: 0 10px 25px rgba(0,0,0,0.25);
    }

    .section h2 {
      margin-top: 0;
      font-size: 24px;
      font-weight: bold;
      text-align: center;
      text-shadow: 1px 1px 3px rgba(0,0,0,0.2);
      margin-bottom: 20px;
    }

    label {
      display: block;
      margin-bottom: 8px;
      font-weight: bold;
      color: inherit;
    }

    input[type="text"], textarea, input[type="file"] {
      width: 100%;
      padding: 10px;
      margin-bottom: 12px;
      border-radius: 8px;
      border: none;
      font-size: 14px;
    }

    button {
      background-color: rgba(255,255,255,0.2);
      color: white;
      border: none;
      padding: 10px 20px;
      border-radius: 8px;
      font-size: 14px;
      cursor: pointer;
      margin-bottom: 10px;
      transition: background 0.3s;
      font-weight: bold;
    }

    button:hover {
      background-color: rgba(255,255,255,0.4);
    }

    .qr-container {
      margin-top: 20px;
      text-align: center;
    }

    /* Ուղեցույց (ավելի փոքր և ներքևում) */
    .guide {
      background-color: #fef9f6;
      border-top: 4px solid #e67e22;
      padding: 15px 20px;
      max-width: 700px;
      margin: 40px auto 0 auto;
      border-radius: 10px;
      font-size: 14px;
      color: #5d4037;
    }

    .guide h2 {
      color: #d35400;
      font-size: 18px;
      margin-top: 0;
    }

    .guide ol {
      padding-left: 20px;
      margin: 10px 0;
    }

    .guide li {
      margin-bottom: 6px;
    }
  </style>
</head>
<body>

  <h1>QR Կոդի Ստեղծում</h1>

  <div class="container">
    <!-- Տեքստից QR -->
    <div class="section">
      <h2>Տեքստից ստանալ QR Կոդ</h2>
      <label for="text">Մուտքագրել տեքստը</label>
      <textarea id="text" rows="4" placeholder="Օրինակ՝ Թեմա 1-ի աշխատանքը"></textarea>
      <button onclick="generateQRCode('text')">Ստեղծել QR</button>
      <div id="qr-text" class="qr-container"></div>
    </div>

    <!-- Հղումից QR -->
    <div class="section">
      <h2>Հղումից ստանալ QR Կոդ</h2>
      <label for="link">Տեղադրել հղումը</label>
      <input type="text" id="link" placeholder="Օրինակ՝ https://drive.google.com/yourfile" />
      <button onclick="generateQRCode('link')">Ստեղծել QR</button>
      <div id="qr-link" class="qr-container"></div>
    </div>

    <!-- Նկարից QR -->
    <div class="section">
      <h2>Նկարից ստանալ QR Կոդ</h2>
      <label for="image">Ընտրել նկար (≤200x200 px)</label>
      <input type="file" id="image" accept="image/*" />
      <button onclick="generateQRCode('image')">Ստեղծել QR</button>
      <div id="qr-image" class="qr-container"></div>
    </div>
  </div>

  <!-- Ուղեցույց -->
  <div class="guide">
    <h2>📌 Ինչպես բեռնել ֆայլը Google Drive-ում և ստանալ հղում</h2>
    <ol>
      <li>Բացեք <a href="https://drive.google.com" target="_blank">Google Drive</a> կայքը։</li>
      <li>Սեղմեք "New" → "File Upload" և ընտրեք ձեր ֆայլը։</li>
      <li>Երբ ֆայլը բեռնվի, սեղմեք աջ կոճակ ֆայլի վրա և ընտրեք <b>"Get link" կամ "share"</b>։</li>
      <li>Փոխեք "Restricted"-ը → "Anyone with the link"։</li>
      <li>Պատճենեք հղումը և տեղադրեք վերևի դաշտում՝ QR ստանալու համար։</li>
    </ol>
  </div>

  <!-- QR գրադարան -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/qrious/4.0.2/qrious.min.js"></script>
  <script>
    function generateQRCode(type) {
      let value = "";
      let containerId = "";

      if (type === "text") {
        value = document.getElementById("text").value.trim();
        containerId = "qr-text";
      } else if (type === "link") {
        value = document.getElementById("link").value.trim();
        containerId = "qr-link";
      } else if (type === "image") {
        const file = document.getElementById("image").files[0];
        containerId = "qr-image";

        if (!file) {
          alert("Խնդրում ենք ընտրել նկար։");
          return;
        }

        const reader = new FileReader();
        reader.onload = function(e) {
          const imgData = e.target.result; // base64 նկար
          createQR(imgData, containerId);
        };
        reader.readAsDataURL(file);
        return;
      }

      if (value === "") {
        alert("Խնդրում ենք լրացնել դաշտը։");
        return;
      }

      createQR(value, containerId);
    }

    function createQR(value, containerId) {
      let qr = new QRious({
        value: value,
        size: 250 // ավելի մեծ QR փոքրիկ նկարների համար
      });

      const container = document.getElementById(containerId);
      container.innerHTML = ""; // Մաքրում է հինը

      const img = qr.image;
      img.style.marginBottom = "10px";

      const downloadBtn = document.createElement("button");
      downloadBtn.textContent = "Ներբեռնել QR կոդը";
      downloadBtn.onclick = function () {
        const link = document.createElement("a");
        link.href = img.src;
        link.download = "qr_code.png";
        link.click();
      };

      container.appendChild(img);
      container.appendChild(downloadBtn);
    }
  </script>

</body>
</html>
