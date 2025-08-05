---
layout: page
title: Personal Horoscope
permalink: /personalhoroscope/
subtitle: Discover your natal blueprint in a single page ✨
---
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <script src="https://unpkg.com/dayjs@1/dayjs.min.js"></script>
  <style>
    body {font-family: system-ui, sans-serif; line-height: 1.5; margin: 0; padding: 0 1rem;}
    h2 {margin-top: 2rem; text-align: center;}
    form {
      max-width: 400px;
      margin: 2rem auto;
      padding: 1rem;
      border: 1px solid #dcdcdc;
      border-radius: 8px;
      background: #fafafa;
    }
    label {display:block; margin-bottom: .25rem; font-weight:600;}
    input {width:100%; padding:.5rem; margin-bottom:1rem; border:1px solid #ccc; border-radius:4px;}
    button {display:block; width:100%; padding:.5rem; background:#3f51b5; color:#fff; border:none; border-radius:4px; cursor:pointer;}
    .result {max-width: 700px; margin:2rem auto; padding:1rem; border:1px solid #eee; border-radius:8px; background:#fff;}
    .chart-placeholder {width:100%; height:300px; display:flex; align-items:center; justify-content:center; background:#f2f2f7; color:#999; border-radius:6px; margin-bottom:1.5rem;}
    .interp {margin-top:1rem;}
  </style>
</head>
<body>
  <h2>🔍 Personal Horoscope</h2>
  <form id="birthForm">
    <label for="birthdate">Birth date (YYYY‑MM‑DD)</label>
    <input type="date" id="birthdate" name="birthdate" required>

    <label for="birthtime">Birth time (24h HH:MM) — optional</label>
    <input type="time" id="birthtime" name="birthtime">

    <button type="submit">Show my horoscope</button>
  </form>

  <div id="result" class="result" style="display:none;"></div>

<script>
// ---------- helper: simple Sun‑sign calc ----------
const sunSignRanges = [
  { sign:'Capricorn',     start:'01-01', end:'01-19' },
  { sign:'Aquarius',      start:'01-20', end:'02-18' },
  { sign:'Pisces',        start:'02-19', end:'03-20' },
  { sign:'Aries',         start:'03-21', end:'04-19' },
  { sign:'Taurus',        start:'04-20', end:'05-20' },
  { sign:'Gemini',        start:'05-21', end:'06-20' },
  { sign:'Cancer',        start:'06-21', end:'07-22' },
  { sign:'Leo',           start:'07-23', end:'08-22' },
  { sign:'Virgo',         start:'08-23', end:'09-22' },
  { sign:'Libra',         start:'09-23', end:'10-22' },
  { sign:'Scorpio',       start:'10-23', end:'11-21' },
  { sign:'Sagittarius',   start:'11-22', end:'12-21' },
  { sign:'Capricorn',     start:'12-22', end:'12-31' }
];

function getSunSign(date){
  const mmdd = date.format('MM-DD');
  return sunSignRanges.find(r=>mmdd>=r.start && mmdd<=r.end)?.sign || 'Unknown';
}

// ---------- very basic interpretations -------------
const interpretations = {
  Aries: `🔥 **Aries** – You’re driven, bold, and always ready to pioneer new territory. Use today’s energy to initiate something meaningful.`,
  Taurus: `🌱 **Taurus** – Patience and practicality shape your world. Ground yourself in sensory pleasures and long‑term plans.`,
  Gemini: `💨 **Gemini** – Curiosity is your compass. Engage in conversations; ideas will spark like lightning.`,
  Cancer: `🌊 **Cancer** – Feelings run deep. Nurture yourself and others; home is your sacred space.`,
  Leo: `☀️ **Leo** – Creativity and confidence illuminate your path. Don’t be afraid to be seen.`,
  Virgo: `🌾 **Virgo** – Details matter. Channel analytical powers into helpful service for yourself and others.`,
  Libra: `⚖️ **Libra** – Harmony seeker. Balance relationships and aesthetics for true fulfilment.`,
  Scorpio: `🦂 **Scorpio** – Intense and transformative. Dive below the surface to find hidden truths.`,
  Sagittarius: `🏹 **Sagittarius** – Adventure calls. Expand your horizons intellectually or literally.`,
  Capricorn: `⛰️ **Capricorn** – Discipline and ambition are your allies. Set a realistic goal and climb.`,
  Aquarius: `🌐 **Aquarius** – Innovator and humanitarian. Embrace originality and community ideals.`,
  Pisces: `✨ **Pisces** – Intuitive and compassionate. Let your imagination swim freely.`
};

// ---------- form handler ---------------------------------
const form = document.getElementById('birthForm');
const resultDiv = document.getElementById('result');

form.addEventListener('submit', e => {
  e.preventDefault();
  const dateVal = document.getElementById('birthdate').value;
  if(!dateVal){ alert('Please enter your birth date'); return; }

  const timeVal = document.getElementById('birthtime').value;
  const dateStr = timeVal ? `${dateVal}T${timeVal}:00Z` : `${dateVal}T00:00:00Z`;
  const birthDate = dayjs(dateStr);

  // basic Sun‑sign demo (replace with Sweph calc later)
  const sunSign = getSunSign(birthDate);

  // render output
  resultDiv.style.display = 'block';
  resultDiv.innerHTML = `
    <h3>📅 Birth data</h3>
    <p>${birthDate.format('YYYY‑MM‑DD HH:mm')} (UTC)</p>

    <h3>🪐 Natal chart (placeholder)</h3>
    <div class="chart-placeholder">SVG natal wheel coming soon</div>

    <h3>📝 Interpretation</h3>
    <div class="interp">${interpretations[sunSign] || 'Coming soon...'}</div>
  `;
});
</script>
</body>
</html>
