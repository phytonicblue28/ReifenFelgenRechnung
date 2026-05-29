<!DOCTYPE html>
<html lang="de">
<head><script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script> 
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Reifenkosten Rechner</title>

<style>

*{
    margin:0;
    padding:0;
    box-sizing:border-box;
    font-family: Arial, sans-serif;
}

body{
    background:#eef2f7;
    padding:30px;
    display:flex;
    justify-content:center;
}

.container{
    max-width:1400px;
    width:100%;
    display:flex;
    gap:20px;
    align-items:flex-start;
}

/* HAUPTBEREICH */
.main{
    flex:1;
    display:flex;
    gap:20px;
    flex-wrap:wrap;
}

/* SIDEBAR */
.sidebar{
    width:320px;
    flex-shrink:0;
}

/* CARDS */
.card{
    background:white;
    padding:25px;
    border-radius:16px;
    box-shadow:0 10px 25px rgba(0,0,0,0.08);
    flex:1;
    min-width:350px;
}

h2{
    text-align:center;
    margin-bottom:20px;
    color:#222;
}

.input-group{
    margin-bottom:15px;
}

label{
    font-weight:bold;
    display:block;
    margin-bottom:6px;
    color:#333;
}

input,
select{
    width:100%;
    padding:12px;
    border-radius:10px;
    border:1px solid #ccc;
    font-size:15px;
}

.button-group{
    display:flex;
    gap:10px;
    margin-top:10px;
}

button{
    flex:1;
    padding:12px;
    border:none;
    border-radius:10px;
    color:white;
    cursor:pointer;
    font-size:15px;
}

.calc-btn{
    background:#007bff;
}

.reset-btn{
    background:#6c757d;
}

.result{
    margin-top:20px;
    text-align:center;
    padding:15px;
    background:#f4f8ff;
    border-radius:12px;
}

.price{
    font-size:28px;
    font-weight:bold;
    color:#007bff;
    margin-top:5px;
}

.marke-buttons button{
    background:#111;
    margin:3px;
    padding:8px 10px;
    border-radius:8px;
    font-size:13px;
}

textarea{
    width:100%;
    margin-top:10px;
    padding:10px;
    border-radius:10px;
    border:1px solid #ccc;
    resize:none;
}

/* FELGEN SIDEBAR */
.felgen-card{
    background:white;
    padding:18px;
    border-radius:14px;
    box-shadow:0 6px 18px rgba(0,0,0,0.08);
}

.felgen-header{
    display:flex;
    justify-content:space-between;
    align-items:center;
}

.toggle-btn{
    width:38px;
    height:38px;
    border:none;
    border-radius:50%;
    background:#007bff;
    color:white;
    font-size:24px;
    cursor:pointer;
}

.rechner-content{
    display:none;
    margin-top:18px;
}

/* SEITENPREIS BOX */
#seitenPreisBox{
    position:fixed;
    bottom:20px;
    right:20px;
    background:white;
    padding:16px 20px;
    border-radius:14px;
    box-shadow:0 10px 25px rgba(0,0,0,0.15);
    border:2px solid #007bff;
    min-width:240px;
    text-align:right;
    z-index:9999;
}

#seitenPreisBox div:first-child{
    font-size:13px;
    color:#555;
    font-weight:bold;
}

#seitenPreisWert{
    font-size:26px;
    color:#007bff;
    font-weight:900;
    margin-top:5px;
}

#kundenreferenzWrapper{

    position:fixed;
    right:20px;
    bottom:170px;

    width:240px;

    z-index:9999;
}

#kundenreferenz{

    width:100%;

    padding:10px 38px 10px 12px;

    border-radius:10px;
    border:1px solid #ccc;

    box-shadow:0 6px 15px rgba(0,0,0,0.15);
}

#clearKundenreferenz{

    position:absolute;

    right:8px;
    top:50%;

    transform:translateY(-50%);

    width:24px;
    height:24px;

    border:none;

    border-radius:50%;

    background:#dc3545;

    color:white;

    font-size:16px;
    font-weight:bold;

    cursor:pointer;

    display:flex;
    align-items:center;
    justify-content:center;

    padding:0;
}
</style>
</head>

<body>

<div class="container">

    <!-- LINKER BEREICH -->
    <div class="main">

        <!-- REIFENRECHNER -->
        <div class="card">

            <h2>Reifenkosten Rechner</h2>

            <div class="input-group">
                <label>Nettopreis pro Reifen (€)</label>
                <input type="number" id="preis">
            </div>

            <div class="input-group">
                <label>Montage</label>
                <select id="montage">
                    <option value="0">Keine Montage</option>
                    <option value="19.5">Montage bis 18 Zoll - 19,50 €</option>
                    <option value="21.5">Montage ab 19 Zoll - 21,50 €</option>
                    <option value="26">Montage Runflat - 26 €</option>
                </select>
            </div>

            <div class="input-group">
                <label>Anzahl</label>
                <select id="anzahl">
                    <option value="1">1</option>
                    <option value="2">2</option>
                    <option value="3">3</option>
                    <option value="4">4</option>
                </select>
            </div>

            <div class="button-group">
                <button class="calc-btn" onclick="berechnen()">
                    Berechnen
                </button>

                <button class="reset-btn" onclick="resetAll()">
                    Reset
                </button>
            </div>

            <div class="result">
                <div>Gesamtpreis ohne MwSt</div>
                <div class="price" id="preisAnzeige">0,00 €</div>
            </div>

        </div>

        <!-- REIFENINFOS -->
        <div class="card">

            <h2>Reifeninformationen</h2>

            <label>Saison</label>
            <select id="saison">
                <option>Sommerreifen</option>
                <option>Winterreifen</option>
                <option>Ganzjahresreifen</option>
            </select>

            <label>Reifengröße</label>
            <input
                type="text"
                id="reifengroesse"
                placeholder="z. B. 215/65 R19"
            >

            <label>Tragfähigkeitsindex</label>
            <input
                type="text"
                id="tragfaehigkeit"
                placeholder="z. B. 99, 102"
            >
            
            <label>Geschwindigkeitsindex</label>
            <input
             type="text"
            id="geschwindigkeit"
             placeholder="z. B. H, V"
            >

            <label>Reifenmarke</label>
            <input
                type="text"
                id="marke"
                placeholder="z. B. Continental"
            >

            <div class="marke-buttons">
                <button onclick="setMarke('Continental')">Continental</button>
                <button onclick="setMarke('Hankook')">Hankook</button>
                <button onclick="setMarke('Michelin')">Michelin</button>
                <button onclick="setMarke('Goodyear')">Goodyear</button>
                <button onclick="setMarke('Bridgestone')">Bridgestone</button>
                <button onclick="setMarke('Pirelli')">Pirelli</button>
                <button onclick="setMarke('Dunlop')">Dunlop</button>
            </div>

            <label>Reifenmodell</label>
            <input
                type="text"
                id="modell"
                placeholder="z. B. UltraContact"
            >

            <h3 style="margin-top:20px;">
                Reifen-Zusammenfassung
            </h3>

            <textarea id="textfeld" rows="4"></textarea>

            <div class="button-group">
                <button class="calc-btn" onclick="copyText()">
                    Kopieren
                </button>

                <button class="reset-btn" onclick="resetReifen()">
                    Reset Infos
                </button>
            </div>

        </div>

    </div>

    <div id="kundenreferenzWrapper">

    <input
        id="kundenreferenz"
        placeholder="z.B. Kunde Müller / Kennzeichen"
    >

    <button id="clearKundenreferenz">
        ×
    </button>

</div>

    <!-- SIDEBAR -->
    <div class="sidebar">

        <div class="felgen-card">

            <div class="felgen-header">
                <h2>Felgen Rechner</h2>

                <button class="toggle-btn" onclick="toggleRechner()">
                    +
                </button>
            </div>

            <div class="rechner-content" id="rechnerContent">

                <div class="input-group">
                    <label>Felgen Einkaufspreis (€)</label>
                    <input
                        type="number"
                        id="felgen_preis"
                        placeholder="z.B. 57,50"
                    >
                </div>

                <div class="input-group">
                    <label>Felgenart</label>

                    <select id="felgen_art">
                        <option>Alufelge</option>
                        <option>Stahlfelge</option>
                    </select>
                </div>

                <div class="input-group">
                    <label>Felgengröße</label>

                    <input
                        type="text"
                        id="felgen_groesse"
                        placeholder="z.B. 8.5 x 19 ET35"
                    >
                </div>

                <div class="input-group">
                    <label>Felgen Marke & Modell</label>

                    <input
                        type="text"
                        id="felgen_marke"
                        placeholder="z.B. BBS, Keskin"
                    >
                </div>

                <div class="input-group">
                    <label>Anzahl</label>

                    <select id="felgen_anzahl">
                        <option value="1">1</option>
                        <option value="2">2</option>
                        <option value="3">3</option>
                        <option value="4">4</option>
                    </select>
                </div>

                <div class="button-group">

                    <button class="calc-btn" onclick="felgenBerechnen()">
                        Berechnen
                    </button>

                    <button class="reset-btn" onclick="felgenReset()">
                        Reset
                    </button>

                </div>

                <div class="result">
                    <div>Gesamtpreis ohne MwSt</div>
                    <div class="price" id="felgen_result">-</div>
                </div>

            </div>

        </div>

    </div>

</div>

<button id="pdfBtn" onclick="createPDF()"
style="
position:fixed;
bottom:110px;
right:20px;
padding:12px 16px;
border:none;
border-radius:12px;
background:#28a745;
color:white;
font-weight:bold;
cursor:pointer;
box-shadow:0 8px 20px rgba(0,0,0,0.2);
">
PDF erstellen
</button> 

<!-- SEITENPREIS -->
<div id="seitenPreisBox">
    <div>Seitenpreis ohne MwSt.</div>
    <div id="seitenPreisWert">0,00 €</div>
</div>

<script>

/* GLOBALE PREISE */
let reifenGesamt = 0;
let felgenGesamt = 0;

/* REIFENRECHNER */

function berechnen(){

    let p = +document.getElementById("preis").value;
    let m = +document.getElementById("montage").value;
    let n = +document.getElementById("anzahl").value;

    let gesamt = (((p + 15) * 1.12) + m) * n;

    reifenGesamt = gesamt;

    document.getElementById("preisAnzeige").innerText =
        gesamt.toFixed(2) + " €";

    updateSeitenPreis();
}

/* FELGENRECHNER */

function toggleRechner(){

    const content = document.getElementById("rechnerContent");
    const btn = event.target;

    if(content.style.display === "block"){

        content.style.display = "none";
        btn.innerHTML = "+";

    } else {

        content.style.display = "block";
        btn.innerHTML = "−";
    }
}

function felgenBerechnen(){

    const preis =
        parseFloat(document.getElementById("felgen_preis").value);

    const anzahl =
        parseInt(document.getElementById("felgen_anzahl").value);

    if(isNaN(preis)) return;

    const gesamt = ((preis + 20) * 1.12) * anzahl;

    felgenGesamt = gesamt;

    document.getElementById("felgen_result").innerText =
        gesamt.toFixed(2) + " €";

    updateSeitenPreis();
}

function felgenReset(){

    felgenGesamt = 0;

    document.getElementById("felgen_preis").value = "";
    document.getElementById("felgen_groesse").value = "";
    document.getElementById("felgen_marke").value = "";
    document.getElementById("felgen_anzahl").value = "1";

    document.getElementById("felgen_result").innerText = "-";

    updateSeitenPreis();
}

/* GESAMTPREIS */

function updateSeitenPreis(){

    const gesamt = reifenGesamt + felgenGesamt;

    document.getElementById("seitenPreisWert").innerText =
        gesamt.toFixed(2) + " €";
}

/* REIFENINFOS */

function setMarke(name){

    document.getElementById("marke").value = name;
    updateText();
}

function updateText(){

    let anzahl =
        document.getElementById("anzahl").value || "1";

    let marke =
        document.getElementById("marke").value.trim();

    let modell =
        document.getElementById("modell").value.trim();

    let saison =
        document.getElementById("saison").value.trim();

    let groesse =
        document.getElementById("reifengroesse").value.trim();

    let speed =
        document.getElementById("geschwindigkeit").value.trim();

    let load =
        document.getElementById("tragfaehigkeit").value.trim();

    // REIFENTEXT
    let reifen =
        (marke ? marke + " " : "") +
        (modell ? modell + " " : "") +
        (saison ? saison + " " : "") +
        (groesse ? groesse + " " : "") +
        (load ? load : "") +
        (speed ? speed : "");

    let text =
        anzahl + "x " + reifen.trim();

    // FELGENDATEN
   let felgenArt =
    document.getElementById("felgen_art").value.trim();

let felgenGroesse =
    document.getElementById("felgen_groesse").value.trim();

let felgenMarke =
    document.getElementById("felgen_marke").value.trim();

let felgenAnzahl =
    document.getElementById("felgen_anzahl").value || "1";

    // FELGEN NUR HINZUFÜGEN WENN ETWAS EINGEGEBEN WURDE
    if(felgenGroesse !== ""){

        text +=
    " und\n" +
    felgenAnzahl + "x " +
    (felgenMarke ? felgenMarke + " " : "") +
    felgenArt + " " +
    felgenGroesse;
    }

    // ABSCHLUSS
    text +=
        "\nkomplett mit Montage, wuchten und Ventilen";

    document.getElementById("textfeld").value = text.trim();
}

/* EVENTS */

document.querySelectorAll("input, select").forEach(el=>{

    el.addEventListener("input", updateText);
    el.addEventListener("change", updateText);
});

/* COPY */

function copyText(){

    let t = document.getElementById("textfeld");

    t.select();

    document.execCommand("copy");
}

/* RESET REIFENINFOS */

function resetReifen(){

    document.querySelectorAll(".card")[1]
    .querySelectorAll("input, select")
    .forEach(el=>{

        if(el.tagName==="SELECT"){
            el.selectedIndex=0;
        } else {
            el.value="";
        }
    });

    document.getElementById("textfeld").value="";

    updateText();
}

/* RESET GESAMT */

function resetAll(){

    document.getElementById("preis").value="";
    document.getElementById("montage").selectedIndex=0;
    document.getElementById("anzahl").selectedIndex=0;

    document.getElementById("preisAnzeige").innerText =
        "0,00 €";

    reifenGesamt = 0;

    updateSeitenPreis();
    updateText();
}

/* FORMAT REIFENGRÖSSE */

document.getElementById("reifengroesse")
.addEventListener("input", function (e) {

    let wert = e.target.value.replace(/\D/g, "");

    if (wert.length > 3) {
        wert = wert.slice(0,3) + "/" + wert.slice(3);
    }

    if (wert.length > 6) {
        wert = wert.slice(0,6) + " R" + wert.slice(6);
    }

    e.target.value = wert;
});

/* GESCHWINDIGKEITSINDEX IMMER GROSS */

document.getElementById("geschwindigkeit")
.addEventListener("input", function (e) {

    e.target.value = e.target.value.toUpperCase();
});

/* START */

updateText();

function createPDF(){

    const { jsPDF } = window.jspdf;
    const doc = new jsPDF();

    let y = 15;

    // 🔵 HEADER
    doc.setFillColor(0, 123, 255);
    doc.rect(0, 0, 210, 25, "F");

    doc.setTextColor(255, 255, 255);
    doc.setFontSize(18);
    doc.text("REIFEN & FELGEN ANGEBOT", 10, 16);
let kundenref = document.getElementById("kundenreferenz").value || "-";

// 🔵 HEADER
doc.setFillColor(0, 123, 255);
doc.rect(0, 0, 210, 30, "F");

doc.setTextColor(255, 255, 255);
doc.setFontSize(18);
doc.text("REIFEN & FELGEN ANGEBOT", 10, 16);

// Kundenreferenz groß rechts
doc.setFontSize(12);
doc.text("Kundenreferenz:", 140, 12);

doc.setFontSize(16);
doc.text(kundenref, 140, 22);
    y = 35;

    // RESET TEXT COLOR
    doc.setTextColor(0, 0, 0);

    // =====================
    // 🟦 REIFEN SECTION
    // =====================
    doc.setFillColor(230, 240, 255);
    doc.rect(10, y - 5, 190, 10, "F");

    doc.setFontSize(13);
    doc.setTextColor(0, 70, 180);
    doc.text("Reifen", 12, y);

    y += 10;
    doc.setTextColor(0, 0, 0);
    doc.setFontSize(11);

    const reifenFelder = [
        ["Marke", "marke"],
        ["Modell", "modell"],
        ["Saison", "saison"],
        ["Größe", "reifengroesse"],
        ["Speed", "geschwindigkeit"],
        ["Load", "tragfaehigkeit"],
        ["Anzahl", "anzahl"]
    ];

    reifenFelder.forEach(f => {
        let val = document.getElementById(f[1]).value || "-";
        doc.text(f[0] + ": " + val, 12, y);
        y += 6;
    });

    y += 5;

    // Montage
    let montage = document.getElementById("montage");
    doc.text("Montage: " + montage.options[montage.selectedIndex].text, 12, y);
    y += 10;

    // =====================
    // 🟦 FELGEN SECTION
    // =====================
    doc.setFillColor(230, 240, 255);
    doc.rect(10, y - 5, 190, 10, "F");

    doc.setTextColor(0, 70, 180);
    doc.setFontSize(13);
    doc.text("Felgen", 12, y);

    y += 10;
    doc.setTextColor(0, 0, 0);
    doc.setFontSize(11);

    const felgenFelder = [
        ["Preis", "felgen_preis"],
        ["Art", "felgen_art"],
        ["Größe", "felgen_groesse"],
        ["Marke/Modell", "felgen_marke"],
        ["Anzahl", "felgen_anzahl"]
    ];

    felgenFelder.forEach(f => {
        let val = document.getElementById(f[1]).value || "-";
        doc.text(f[0] + ": " + val, 12, y);
        y += 6;
    });

    y += 10;

    // =====================
    // 💰 GESAMTPREIS
    // =====================
    doc.setFillColor(0, 123, 255);
    doc.rect(10, y - 5, 190, 15, "F");

    doc.setTextColor(255, 255, 255);
    doc.setFontSize(14);

    doc.text(
        "Gesamtpreis ohne MwSt.: " + (reifenGesamt + felgenGesamt).toFixed(2) + " €",
        12,
        y + 5
    );

    doc.save("angebot_reifen_felgen.pdf");
}
document.getElementById("clearKundenreferenz")
.addEventListener("click", function(){

    document.getElementById("kundenreferenz").value = "";

});
</script>

</body>
</html>
