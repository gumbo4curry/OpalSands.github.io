<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Dunedin Bar Crawl Trail · Opal Sands Resort</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;700;900&family=Lato:wght@300;400;700&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"/>
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<style>
  :root {
    --navy:   #0D2B45;
    --navy2:  #122f4d;
    --gold:   #C9A84C;
    --gold2:  #e0c06a;
    --teal:   #2E7D8E;
    --cream:  #FAF6EF;
    --rule:   #D9CDB8;
    --text:   #1a1a1a;
    --muted:  #666;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    font-family: 'Lato', sans-serif;
    background: var(--cream);
    color: var(--text);
    min-height: 100vh;
  }

  /* ── HEADER ── */
  header {
    background: var(--navy);
    padding: 0;
    position: relative;
    overflow: hidden;
  }
  header::before {
    content: '';
    position: absolute;
    inset: 0;
    background: repeating-linear-gradient(
      45deg,
      transparent,
      transparent 40px,
      rgba(201,168,76,0.04) 40px,
      rgba(201,168,76,0.04) 80px
    );
  }
  .gold-rule { height: 3px; background: var(--gold); width: 100%; }
  .header-inner {
    max-width: 900px;
    margin: 0 auto;
    padding: 28px 20px 24px;
    text-align: center;
    position: relative;
    z-index: 1;
  }
  .resort-tag {
    font-family: 'Lato', sans-serif;
    font-size: 10px;
    font-weight: 700;
    letter-spacing: 3px;
    color: var(--gold);
    text-transform: uppercase;
    margin-bottom: 10px;
  }
  h1 {
    font-family: 'Playfair Display', serif;
    font-size: clamp(26px, 6vw, 44px);
    font-weight: 900;
    color: #fff;
    line-height: 1.1;
    margin-bottom: 8px;
  }
  .header-sub {
    font-family: 'Lato', sans-serif;
    font-size: 13px;
    color: var(--rule);
    letter-spacing: 1px;
    font-weight: 300;
  }

  /* ── MAP ── */
  #map {
    width: 100%;
    height: 420px;
    border-top: none;
    border-bottom: 3px solid var(--gold);
    z-index: 1;
  }

  /* Custom Leaflet popup */
  .leaflet-popup-content-wrapper {
    border-radius: 4px;
    padding: 0;
    overflow: hidden;
    box-shadow: 0 8px 30px rgba(0,0,0,0.2);
    border-bottom: 3px solid var(--gold);
  }
  .leaflet-popup-content { margin: 0; width: 220px !important; }
  .popup-header {
    background: var(--navy);
    padding: 10px 14px 8px;
  }
  .popup-num {
    font-family: 'Playfair Display', serif;
    font-size: 11px;
    color: var(--gold);
    letter-spacing: 2px;
    text-transform: uppercase;
  }
  .popup-name {
    font-family: 'Playfair Display', serif;
    font-size: 15px;
    color: #fff;
    line-height: 1.3;
    margin-top: 2px;
  }
  .popup-body { padding: 10px 14px 12px; background: #fff; }
  .popup-tag {
    font-size: 10px;
    color: var(--teal);
    font-weight: 700;
    letter-spacing: 0.5px;
    text-transform: uppercase;
    margin-bottom: 6px;
  }
  .popup-desc { font-size: 11.5px; color: #444; line-height: 1.5; margin-bottom: 8px; }
  .popup-must-label {
    font-size: 9px;
    font-weight: 700;
    color: var(--gold);
    letter-spacing: 1px;
    text-transform: uppercase;
    margin-bottom: 3px;
  }
  .popup-must { font-size: 11px; color: var(--navy); font-style: italic; }
  .popup-hours { font-size: 10px; color: var(--muted); margin-top: 8px; border-top: 1px solid #eee; padding-top: 6px; }

  /* ── TRAIL INTRO ── */
  .intro-strip {
    background: var(--navy2);
    padding: 16px 20px;
    text-align: center;
  }
  .intro-strip p {
    max-width: 700px;
    margin: 0 auto;
    font-size: 13px;
    color: var(--rule);
    line-height: 1.7;
    font-weight: 300;
  }

  /* ── STOPS GRID ── */
  .stops-section {
    max-width: 900px;
    margin: 0 auto;
    padding: 36px 16px 16px;
  }
  .section-label {
    font-family: 'Lato', sans-serif;
    font-size: 10px;
    font-weight: 700;
    letter-spacing: 3px;
    color: var(--gold);
    text-transform: uppercase;
    margin-bottom: 20px;
    display: flex;
    align-items: center;
    gap: 12px;
  }
  .section-label::after {
    content: '';
    flex: 1;
    height: 1px;
    background: var(--rule);
  }

  .stops-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
    gap: 16px;
  }

  .stop-card {
    background: #fff;
    border: 1px solid var(--rule);
    border-bottom: 3px solid var(--gold);
    border-radius: 3px;
    overflow: hidden;
    cursor: pointer;
    transition: transform 0.2s, box-shadow 0.2s;
  }
  .stop-card:hover {
    transform: translateY(-3px);
    box-shadow: 0 10px 30px rgba(13,43,69,0.12);
  }
  .stop-card-header {
    background: var(--navy);
    padding: 10px 14px;
    display: flex;
    align-items: center;
    gap: 12px;
  }
  .stop-num {
    font-family: 'Playfair Display', serif;
    font-size: 22px;
    font-weight: 900;
    color: var(--gold);
    line-height: 1;
    min-width: 32px;
  }
  .stop-title-block {}
  .stop-name {
    font-family: 'Playfair Display', serif;
    font-size: 14px;
    color: #fff;
    line-height: 1.3;
  }
  .stop-time {
    font-size: 10px;
    color: var(--rule);
    margin-top: 2px;
    font-weight: 300;
  }
  .stop-body { padding: 12px 14px; }
  .stop-tag {
    font-size: 9.5px;
    color: var(--teal);
    font-weight: 700;
    letter-spacing: 0.5px;
    text-transform: uppercase;
    margin-bottom: 7px;
  }
  .stop-desc {
    font-size: 12px;
    color: #444;
    line-height: 1.6;
    margin-bottom: 10px;
  }
  .must-label {
    font-size: 8.5px;
    font-weight: 700;
    letter-spacing: 1.5px;
    color: var(--gold);
    text-transform: uppercase;
    margin-bottom: 3px;
  }
  .must-items {
    font-size: 11.5px;
    color: var(--navy);
    font-style: italic;
    margin-bottom: 10px;
  }
  .stop-hours {
    font-size: 10px;
    color: var(--muted);
    border-top: 1px solid #f0ece4;
    padding-top: 8px;
  }
  .stop-hours .warn { color: #c0392b; font-weight: 700; }

  /* ── TIPS BAR ── */
  .tips-bar {
    background: var(--navy);
    margin-top: 36px;
  }
  .tips-inner {
    max-width: 900px;
    margin: 0 auto;
    padding: 24px 16px;
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    gap: 20px;
  }
  .tip-item {}
  .tip-icon-label {
    font-size: 9px;
    font-weight: 700;
    letter-spacing: 2px;
    color: var(--gold);
    text-transform: uppercase;
    margin-bottom: 5px;
  }
  .tip-text { font-size: 12px; color: var(--rule); line-height: 1.6; font-weight: 300; }

  /* ── QR SECTION ── */
  .qr-section {
    background: var(--cream);
    border-top: 1px solid var(--rule);
  }
  .qr-inner {
    max-width: 900px;
    margin: 0 auto;
    padding: 32px 16px;
    display: flex;
    align-items: center;
    gap: 32px;
    flex-wrap: wrap;
    justify-content: center;
  }
  .qr-box {
    background: #fff;
    border: 2px solid var(--gold);
    padding: 16px;
    border-radius: 4px;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 10px;
    min-width: 160px;
  }
  #qr-canvas canvas { display: block; }
  .qr-label {
    font-size: 9px;
    letter-spacing: 2px;
    color: var(--navy);
    text-transform: uppercase;
    font-weight: 700;
    text-align: center;
  }
  .qr-info { max-width: 400px; }
  .qr-info h3 {
    font-family: 'Playfair Display', serif;
    font-size: 20px;
    color: var(--navy);
    margin-bottom: 8px;
  }
  .qr-info p { font-size: 13px; color: var(--muted); line-height: 1.7; }
  .qr-url {
    margin-top: 10px;
    font-size: 11px;
    color: var(--teal);
    word-break: break-all;
    font-family: monospace;
    background: #eef7f9;
    padding: 6px 10px;
    border-radius: 3px;
    border-left: 3px solid var(--teal);
  }

  /* ── FOOTER ── */
  footer {
    background: var(--navy);
    padding: 18px 20px;
    text-align: center;
  }
  footer p { font-size: 11px; color: var(--rule); font-weight: 300; line-height: 1.8; }
  footer strong { color: var(--gold); font-weight: 700; }

  /* ── CUSTOM MARKER ── */
  .custom-marker {
    background: var(--navy);
    border: 2.5px solid var(--gold);
    border-radius: 50% 50% 50% 0;
    transform: rotate(-45deg);
    width: 32px; height: 32px;
    display: flex; align-items: center; justify-content: center;
    box-shadow: 0 3px 10px rgba(0,0,0,0.3);
  }
  .marker-num {
    transform: rotate(45deg);
    color: var(--gold);
    font-family: 'Playfair Display', serif;
    font-size: 12px;
    font-weight: 900;
    line-height: 1;
  }

  /* Active stop card */
  .stop-card.active { box-shadow: 0 0 0 2px var(--gold), 0 10px 30px rgba(13,43,69,0.15); }

  @media (max-width: 600px) {
    #map { height: 320px; }
    .stops-grid { grid-template-columns: 1fr; }
    .qr-inner { flex-direction: column; }
  }
</style>
</head>
<body>

<!-- HEADER -->
<div class="gold-rule"></div>
<header>
  <div class="header-inner">
    <div class="resort-tag">Opal Sands Resort &nbsp;·&nbsp; Clearwater Beach, FL</div>
    <h1>Dunedin Bar Crawl Trail</h1>
    <p class="header-sub">A Curated Evening Experience &nbsp;·&nbsp; 7 Stops &nbsp;·&nbsp; Concierge Recommended</p>
  </div>
</header>
<div class="gold-rule"></div>

<!-- MAP -->
<div id="map"></div>

<!-- INTRO -->
<div class="intro-strip">
  <p>Dunedin is one of Florida's most walkable and celebrated craft destinations — a charming Scottish-founded coastal town packed with award-winning breweries, vibrant cocktail bars, and Gulf sunset views. This hand-picked trail covers 7 stops in a logical walking loop. Pace yourself, stay hydrated, and enjoy every moment.</p>
</div>

<!-- STOPS -->
<div class="stops-section">
  <div class="section-label">The Trail</div>
  <div class="stops-grid" id="stops-grid"></div>
</div>

<!-- TIPS -->
<div class="tips-bar">
  <div class="gold-rule"></div>
  <div class="tips-inner">
    <div class="tip-item">
      <div class="tip-icon-label">🚗 &nbsp;Getting There</div>
      <p class="tip-text">~20 min drive from Clearwater Beach via Alt 19 N. Free parking near the marina and on side streets off Broadway.</p>
    </div>
    <div class="tip-item">
      <div class="tip-icon-label">📱 &nbsp;Rideshare</div>
      <p class="tip-text">Uber & Lyft are active in Dunedin. Book your return ride from Hi-Fi Rooftop — do not drive after the crawl.</p>
    </div>
    <div class="tip-item">
      <div class="tip-icon-label">🐾 &nbsp;Dog Friendly</div>
      <p class="tip-text">HOB Brewing, Cueni, and DHOB all welcome well-behaved dogs — a great trail for guests traveling with pets.</p>
    </div>
    <div class="tip-item">
      <div class="tip-icon-label">🏴󠁧󠁢󠁳󠁣󠁴󠁿 &nbsp;Fun Fact</div>
      <p class="tip-text">Dunedin was founded by Scottish settlers in the 1870s and named after Edinburgh's Gaelic name. Highland Games held here every spring!</p>
    </div>
  </div>
  <div class="gold-rule"></div>
</div>

<!-- QR SECTION -->
<div class="qr-section">
  <div class="qr-inner">
    <div class="qr-box">
      <div id="qr-canvas"></div>
      <div class="qr-label">Scan to Share</div>
    </div>
    <div class="qr-info">
      <h3>Share This Guide</h3>
      <p>Scan the QR code to open this interactive trail on any smartphone — perfect for passing directly to guests at check-in or printing on your concierge desk card.</p>
      <div class="qr-url" id="page-url">Loading URL...</div>
    </div>
  </div>
</div>

<!-- FOOTER -->
<footer>
  <p><strong>Curated by Your Concierge Team &nbsp;·&nbsp; Opal Sands Resort</strong><br>
  Clearwater Beach, Florida &nbsp;·&nbsp; For assistance, visit the Concierge Desk or call the front desk.</p>
</footer>

<!-- QR CODE LIBRARY (inline, no CDN needed) -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>

<script>
// ── STOP DATA ────────────────────────────────────────────────────────────────
const stops = [
  {
    num: "01", name: "Bon Appétit Marina Bar",
    tag: "Upscale Waterfront · Live Piano Weekends",
    time: "Start: 4:00 PM · ~45 min",
    hours: "Open until 10 PM daily",
    desc: "Kick off the evening at the iconic Dunedin Marina waterfront. Stunning Gulf views, an elegant atmosphere, and weekend live piano — a refined way to set the tone for the night. Great spot for a pre-crawl bite.",
    must: "Blackened grouper · Craft cocktails · Smoked fish starter",
    lat: 28.013257, lng: -82.793209
  },
  {
    num: "02", name: "Dunedin House of Beer",
    tag: "Dive Taphouse · Massive Draft List · Dog Friendly",
    time: "4:50 PM · ~40 min",
    hours: "Open until 3 AM daily",
    desc: "A legendary Broadway taphouse with one of the most impressive rotating draft lists in Pinellas County. Casual, game-filled, and famously welcoming. Sister venue to HOB Brewing directly across the street.",
    must: "Rotating guest taps · HOB house drafts · Canned selections",
    lat: 28.014042, lng: -82.789505
  },
  {
    num: "03", name: "HOB Brewing Co.",
    tag: "German Beer Hall · Food Trucks · Family & Dog Friendly",
    time: "5:35 PM · ~40 min",
    hours: "Open until Midnight (Fri/Sat)",
    desc: "Step across the street for HOB's expansive covered outdoor patio with a German biergarten feel. Three pour sizes let you sample multiple beers. Regularly hosts rotating food trucks.",
    must: "Peanut Butter Blonde · Rotating sours · Splashin' Around IPA",
    lat: 28.013741, lng: -82.789019
  },
  {
    num: "04", name: "Cueni Brewing Co.",
    tag: "Hidden Gem · Experimental Brews · 4.8★ Rated",
    time: "6:20 PM · ~35 min",
    hours: "Open until 11 PM (Fri/Sat)",
    desc: "One of Dunedin's best-kept secrets and highest-rated breweries in all of Pinellas County. Known for inventive small-batch creations and an incredibly warm community atmosphere.",
    must: "Pickle Beer · Rotating experimental batches · Seasonal specialties",
    lat: 28.014677, lng: -82.788966
  },
  {
    num: "05", name: "Señor Rita's Tequilería",
    tag: "Tequila Bar · Hand-Crafted Margaritas · Late Night",
    time: "7:00 PM · ~45 min",
    hours: "Open until 1 AM (Fri/Sat) · ⚠ CLOSED WEDNESDAYS",
    desc: "Time to shift gears — Señor Rita's brings vibrant energy, swing seats, colorful photo walls, and the best margaritas in Dunedin. Staff hand-craft all fruit syrups fresh daily.",
    must: "House frozen margaritas · Fresh-syrup specialty cocktails · Late-night tacos",
    lat: 28.013936, lng: -82.789512,
    warn: true
  },
  {
    num: "06", name: "Sonder Social Club",
    tag: "Seasonal Cocktail Bar · Rotating Themes · Upscale Casual",
    time: "7:50 PM · ~45 min",
    hours: "Open until Midnight (Fri/Sat 2 AM)",
    desc: "One of Dunedin's most stylish cocktail bars. Sonder rotates its entire theme, décor, and cocktail menu seasonally — so every visit feels completely fresh. Beautifully curated drinks and impressive happy hour.",
    must: "Seasonal signature cocktails · Ahi tuna poke · Craft Old Fashioned",
    lat: 28.014283, lng: -82.788205
  },
  {
    num: "07", name: "Hi-Fi Rooftop Bar",
    tag: "Grand Finale · Gulf Sunset Views · Craft Cocktails",
    time: "8:45 PM · ~60 min",
    hours: "Open until 11 PM (Fri/Sat)",
    desc: "End the night in style atop the Fenway Hotel. Hi-Fi's rooftop delivers panoramic Gulf views, a signature nightly sunset tradition, and elevated craft cocktails — the perfect wind-down after a full evening.",
    must: "Craft cocktails · Small bites · Ask about the sunset tradition",
    lat: 28.005986, lng: -82.791369
  }
];

// ── MAP INIT ─────────────────────────────────────────────────────────────────
const map = L.map('map', { zoomControl: true, scrollWheelZoom: false });
L.tileLayer('https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png', {
  attribution: '© OpenStreetMap, © CARTO',
  subdomains: 'abcd', maxZoom: 19
}).addTo(map);

const markers = [];
const bounds = [];

stops.forEach((stop, i) => {
  const icon = L.divIcon({
    className: '',
    html: `<div class="custom-marker"><span class="marker-num">${stop.num}</span></div>`,
    iconSize: [32, 32],
    iconAnchor: [16, 32],
    popupAnchor: [0, -36]
  });

  const warnHtml = stop.warn ? `<span class="warn">⚠ Closed Wednesdays</span> · ` : '';
  const popup = L.popup({ maxWidth: 240, className: '' }).setContent(`
    <div class="popup-header">
      <div class="popup-num">Stop ${stop.num}</div>
      <div class="popup-name">${stop.name}</div>
    </div>
    <div class="popup-body">
      <div class="popup-tag">${stop.tag}</div>
      <div class="popup-desc">${stop.desc}</div>
      <div class="popup-must-label">Must Try</div>
      <div class="popup-must">${stop.must}</div>
      <div class="popup-hours">${warnHtml}${stop.hours}</div>
    </div>
  `);

  const marker = L.marker([stop.lat, stop.lng], { icon }).addTo(map).bindPopup(popup);
  markers.push(marker);
  bounds.push([stop.lat, stop.lng]);

  marker.on('click', () => {
    document.querySelectorAll('.stop-card').forEach(c => c.classList.remove('active'));
    const card = document.getElementById(`card-${i}`);
    if (card) {
      card.classList.add('active');
      card.scrollIntoView({ behavior: 'smooth', block: 'center' });
    }
  });
});

// Draw route polyline
const routeCoords = stops.map(s => [s.lat, s.lng]);
L.polyline(routeCoords, { color: '#C9A84C', weight: 2.5, opacity: 0.7, dashArray: '6 6' }).addTo(map);
map.fitBounds(bounds, { padding: [30, 30] });

// ── STOP CARDS ───────────────────────────────────────────────────────────────
const grid = document.getElementById('stops-grid');
stops.forEach((stop, i) => {
  const card = document.createElement('div');
  card.className = 'stop-card';
  card.id = `card-${i}`;
  const warnHtml = stop.warn ? `<span class="warn">⚠ Closed Wednesdays &nbsp;·&nbsp;</span>` : '';
  card.innerHTML = `
    <div class="stop-card-header">
      <div class="stop-num">${stop.num}</div>
      <div class="stop-title-block">
        <div class="stop-name">${stop.name}</div>
        <div class="stop-time">${stop.time}</div>
      </div>
    </div>
    <div class="stop-body">
      <div class="stop-tag">${stop.tag}</div>
      <div class="stop-desc">${stop.desc}</div>
      <div class="must-label">Must Try</div>
      <div class="must-items">${stop.must}</div>
      <div class="stop-hours">${warnHtml}${stop.hours}</div>
    </div>
  `;
  card.addEventListener('click', () => {
    map.setView([stop.lat, stop.lng], 16, { animate: true });
    markers[i].openPopup();
    document.querySelectorAll('.stop-card').forEach(c => c.classList.remove('active'));
    card.classList.add('active');
    window.scrollTo({ top: 0, behavior: 'smooth' });
  });
  grid.appendChild(card);
});

// ── QR CODE ──────────────────────────────────────────────────────────────────
const pageUrl = window.location.href;
document.getElementById('page-url').textContent = pageUrl;

if (typeof QRCode !== 'undefined') {
  new QRCode(document.getElementById('qr-canvas'), {
    text: pageUrl,
    width: 130, height: 130,
    colorDark: '#0D2B45',
    colorLight: '#ffffff',
    correctLevel: QRCode.CorrectLevel.M
  });
} else {
  document.getElementById('qr-canvas').innerHTML =
    '<p style="font-size:11px;color:#999;text-align:center;padding:10px;">QR code loads<br>when hosted online</p>';
}
</script>
</body>
</html>
