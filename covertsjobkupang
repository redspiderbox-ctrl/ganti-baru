<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<title>Layout Foto A4 → PDF</title>

<!-- jsPDF -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<!-- html2canvas -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>

<style>
body {
    font-family: Arial, sans-serif;
    background:#f2f2f2;
    padding:20px;
}

.controls {
    background:#fff;
    padding:15px;
    border-radius:6px;
    margin-bottom:20px;
}

button {
    padding:8px 14px;
    margin-right:10px;
    cursor:pointer;
}

.page {
    width:21cm;
    height:29.7cm;
    background:white;
    margin:20px auto;
    padding:1cm;
    box-shadow:0 0 8px rgba(0,0,0,0.3);
    box-sizing:border-box;
    page-break-after:always;
}

.grid {
    display:grid;
    grid-template-columns: repeat(3, 1fr);
    grid-template-rows: repeat(3, 1fr);
    gap:0.5cm;
    height:100%;
}

.photo {
    border:2px solid black;
    display:flex;
    align-items:center;
    justify-content:center;
    overflow:hidden;
}

.photo img {
    width:100%;
    height:100%;
    object-fit:cover;
}
</style>
</head>

<body>

<div class="controls">
    <input type="file" id="upload" multiple accept="image/*">
    <button onclick="generatePDF()">Download PDF</button>
</div>

<div id="preview"></div>

<script>
const upload = document.getElementById("upload");
const preview = document.getElementById("preview");

upload.addEventListener("change", () => {
    preview.innerHTML = "";
    const files = [...upload.files];
    const perPage = 9;

    for (let i = 0; i < files.length; i += perPage) {
        const page = document.createElement("div");
        page.className = "page";

        const grid = document.createElement("div");
        grid.className = "grid";

        files.slice(i, i + perPage).forEach(file => {
            const reader = new FileReader();
            reader.onload = e => {
                const box = document.createElement("div");
                box.className = "photo";

                const img = document.createElement("img");
                img.src = e.target.result;

                box.appendChild(img);
                grid.appendChild(box);
            };
            reader.readAsDataURL(file);
        });

        page.appendChild(grid);
        preview.appendChild(page);
    }
});

async function generatePDF() {
    const { jsPDF } = window.jspdf;
    const pdf = new jsPDF("p", "cm", "a4");

    const pages = document.querySelectorAll(".page");

    for (let i = 0; i < pages.length; i++) {
        const canvas = await html2canvas(pages[i], { scale:2 });
        const imgData = canvas.toDataURL("image/jpeg", 1.0);

        if (i > 0) pdf.addPage();
        pdf.addImage(imgData, "JPEG", 0, 0, 21, 29.7);
    }

    pdf.save("layout-foto.pdf");
}
</script>

</body>
</html>
