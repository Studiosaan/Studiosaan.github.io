---
layout: page
title: Personal Horoscope
permalink: /personalhoroscope/
subtitle: Discover your natal blueprint in a single page ✨
---
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <script src="https://unpkg.com/dayjs@1/dayjs.min.js"></script>
  <style>
    /* ---------- CSS Reset & Root ---------- */
    :root {
      --primary: #3f51b5;
      --bg: #fafafa;
      --bg-dark: #1c1c1e;
      --fg-dark: #e5e5e7;
      --border: #e0e0e0;
      --radius: 10px;
    }

    html {
      scroll-behavior: smooth;
    }

    body {
      margin: 0;
      font-family: "Inter", system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, "Noto Sans", sans-serif;
      line-height: 1.6;
      background: var(--bg);
      color: #333;
      display: flex;
      flex-direction: column;
      min-height: 100vh;
      padding-inline: 1rem;
    }

    @media (prefers-color-scheme: dark) {
      body { background: var(--bg-dark); color: var(--fg-dark); }
      .card, .result { background: #2a2a2d; border-color: #3a3a3d; }
      input, button { color: var(--fg-dark); }
    }

    /* ---------- Typography ---------- */
    h1, h2, h3 { font-weight: 700; margin: 1.5rem 0 .75rem; text-align: center; }
    h2 { font-size: clamp(1.8rem, 2.5vw + .5rem, 2.4rem); }
    h3 { font-size: clamp(1.25rem, 2vw + .25rem, 1.5rem); text-align: left; }

    /* ---------- Utility ---------- */
    .mx-auto { margin-inline: auto; }
    .w-full  { width: 100%; }

    /* ---------- Card Styles ---------- */
    .card {
      max-width: 480px;
      padding: 1rem 1.25rem;
      border: 1px solid var(--border);
      border-radius: var(--radius);
      background: #fff;
      box-shadow: 0 2px 4px rgba(0,0,0,.05);
    }

    /* ---------- Form ---------- */
    form label { display: block; margin-bottom: .35rem; font-weight: 600; }
    form input {
      width: 100%;
      padding: .6rem .8rem;
      margin-bottom: 1.1rem;
      border: 1px solid #cfcfcf;
      border-radius: var(--radius);
      font-size: .95rem;
    }
    form button {
      width: 100%;
      padding: .7rem;
      background: var(--primary);
      color: #fff;
      border: none;
      border-radius: var(--radius);
      font-size: .95rem;
      cursor: pointer;
      transition: background .2s ease;
    }
    form button:hover { background: #2f3fa0; }

    /* ---------- Result Section ---------- */
    .result {
      max-width: 700px;
      margin-block: 2rem 3rem;
      padding: 1.5rem 1.25rem;
      border: 1px solid var(--border);
      border-radius: var(--radius);
      background: #fff;
      box-shadow: 0 4px 6px rgba(0,0,0,.06);
      animation: fadeIn .4s ease;
    }

    .chart-placeholder {
      width: 100%; height: 320px;
      display:flex; align-items:center; justify-content:center;
      background: #f2f2f7; color:#888;
      border-radius: var(--radius);
      font-size: 1rem;
    }

    @keyframes fadeIn { from{opacity:0; transform:translateY(10px);} to{opacity:1;} }

    /* ---------- Responsive Adjustments ---------- */
    @media (min-width: 768px) {
      form.card { display: flex; flex-wrap: wrap; gap: 1rem; align-items: flex-end; }
      form .field { flex: 1 1 200px; }
      form button { flex: 1 1 150px; margin-bottom: 0; }
    }
  </style>
</head>
<body>
  <h2>🔍 Personal Horoscope</h2>

  <!-- Input Card -->
  <form id="birthForm" class="card mx-auto">
    <div class="field w-full">
      <label for="birthdate">Birth date (YYYY‑MM‑DD)</label>
      <input type="date" id="birthdate" name="birthdate" required />
    </div>
    <div class="field w-full">
      <label for="birthtime">Birth time (24h HH:MM) — optional</label>
      <input type="time" id="birthtime" name="birthtime" />
    </div>
    <button type="submit">Show my horoscope</button>
  </form>

  <!-- Result -->
  <div id="result" class="result" style="display:none;"></div>

<script>
// ---------- helper: simple Sun‑sign calc ----------
const sunSignRanges=[{sign:"Capricorn",start:"01-01",end:"01-19"},{sign:"Aquarius",start:"01-20",end:"02-18"},{sign:"Pisces",start:"02-19",end:"03-20"},{sign:"Aries",start:"03-21",end:"04-19"},{sign:"Taurus",start:"04-20",end:"05-20"},{sign:"Gemini",start:"05-21",end:"06-20"},{sign:"Cancer",start:"06-21",end:"07-22"},{sign:"Leo",start:"07-23",end:"08-22"},{sign:"Virgo",start:"08-23",end:"09-22"},{sign:"Libra",start:"09-23",end:"10-22"},{sign:"Scorpio",start:"10-23",end:"11-21"},{sign:"Sagittarius",start:"11-22",end:"12-21"},{sign:"Capricorn",start:"12-22",end:"12-31"}];
const interpretations={Aries:"🔥 <strong>Aries</strong> – You're driven, bold, and always ready to pioneer new territory. Use today's energy to initiate something meaningful.",Taurus:"🌱 <strong>Taurus</strong> – Patience and practicality shape your world. Ground yourself in sensory pleasures and long‑term plans.",Gemini:"💨 <strong>Gemini</strong> – Curiosity is your compass. Engage in conversations; ideas will spark like lightning.",Cancer:"🌊 <strong>Cancer</strong> – Feelings run deep. Nurture yourself and others; home is your sacred space.",Leo:"☀️ <strong>Leo</strong> – Creativity and confidence illuminate your path. Don't be afraid to be seen.",Virgo:"🌾 <strong>Virgo</strong> – Details matter. Channel analytical powers into helpful service for yourself and others.",Libra:"⚖️ <strong>Libra</strong> – Harmony seeker. Balance relationships and aesthetics for true fulfilment.",Scorpio:"🦂 <strong>Scorpio</strong> – Intense and transformative. Dive below the surface to find hidden truths.",Sagittarius:"🏹 <strong>Sagittarius</strong> – Adventure calls. Expand your horizons intellectually or literally.",Capricorn:"⛰️ <strong>Capricorn</strong> – Discipline and ambition are your allies. Set a realistic goal and climb.",Aquarius:"🌐 <strong>Aquarius</strong> – Innovator and humanitarian. Embrace originality and community ideals.",Pisces:"✨ <strong>Pisces</strong> – Intuitive and compassionate. Let your imagination swim freely."};
const form=document.getElementById("birthForm"),resultDiv=document.getElementById("result");function getSunSign(e){const t=e.format("MM-DD");return(sunSignRanges.find(e=>t>=e.start&&t<=e.end)||{}).sign||"Unknown"}form.addEventListener("submit",e=>{e.preventDefault();const t=document.getElementById("birthdate").value;if(!t)return void alert("Please enter your birth date");const i=document.getElementById("birthtime").value,n=i?`${t}T${i}:00Z`:`${t}T00:00:00Z`,r=dayjs(n),o=getSunSign(r);resultDiv.style.display="block",resultDiv.innerHTML=`<h3>📅 Birth data</h3><p>${r.format("YYYY‑MM‑DD HH:mm")} (UTC)</p><h3>🪐 Natal chart</h3><div class="chart-placeholder">SVG natal wheel coming soon</div><h3>📝 Interpretation</h3><div class="interp">${interpretations[o]||"Coming soon..."}</div>`});
</script>
</body>
</html>
