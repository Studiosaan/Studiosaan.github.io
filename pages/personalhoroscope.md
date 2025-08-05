---
layout: page
title: Personal Horoscope
permalink: /personalhoroscope/
subtitle: Discover your natal blueprint in a single page ✨
---

<!-- 본문만: Beautiful Jekyll 헤더·푸터를 그대로 사용합니다 -->

<style>
  /* ---------- Root Palette ---------- */
  :root{
    --primary:#3f51b5;
    --border:#e0e0e0;
    --radius:10px;
  }
  /* Let Beautiful‑Jekyll keep its own light/dark background. Only tune components */
  h2{text-align:center;font-weight:700;font-size:clamp(1.8rem,2.5vw + .5rem,2.4rem);margin:2rem 0 1rem}
  .mx-auto{margin-inline:auto}.w-full{width:100%}

  /* Card */
  .card{max-width:480px;padding:1rem 1.25rem;border:1px solid var(--border);border-radius:var(--radius);background:#fff;box-shadow:0 2px 4px rgba(0,0,0,.05)}

  /* Form */
  form label{display:block;margin-bottom:.35rem;font-weight:600}
  form input{width:100%;padding:.6rem .8rem;margin-bottom:1.1rem;border:1px solid #cfcfcf;border-radius:var(--radius);font-size:.95rem}
  form button{width:100%;padding:.7rem;background:var(--primary);color:#fff;border:none;border-radius:var(--radius);font-size:.95rem;cursor:pointer;transition:background .2s ease}
  form button:hover{background:#2d3aa0}

  /* Result Box */
  .result{max-width:700px;margin:2rem auto 3rem;padding:1.5rem 1.25rem;border:1px solid var(--border);border-radius:var(--radius);background:#fff;box-shadow:0 4px 6px rgba(0,0,0,.06);animation:fadeIn .4s ease}
  .chart-placeholder{width:100%;height:320px;display:flex;align-items:center;justify-content:center;background:#f5f5f5;color:#888;border-radius:var(--radius)}
  @keyframes fadeIn{from{opacity:0;transform:translateY(10px)}to{opacity:1}}

  /* Responsive tweak */
  @media(min-width:768px){form.card{display:flex;flex-wrap:wrap;gap:1rem;align-items:flex-end}form .field{flex:1 1 200px}form button{flex:1 1 150px;margin-bottom:0}}
</style>

<h2>🔍 Personal Horoscope</h2>

<form id="birthForm" class="card mx-auto">
  <div class="field w-full">
    <label for="birthdate">Birth date (YYYY‑MM‑DD)</label>
    <input type="date" id="birthdate" name="birthdate" required />
  </div>
  <div class="field w-full">
    <label for="birthtime">Birth time (24h HH:MM) — optional</label>
    <input type="time" id="birthtime" name="birthtime" />
  </div>
  <button type="submit">Show my horoscope</button>
</form>

<div id="result" class="result" style="display:none;"></div>

<script src="https://unpkg.com/dayjs@1/dayjs.min.js"></script>
<script>
const sunSignRanges=[{sign:"Capricorn",start:"01-01",end:"01-19"},{sign:"Aquarius",start:"01-20",end:"02-18"},{sign:"Pisces",start:"02-19",end:"03-20"},{sign:"Aries",start:"03-21",end:"04-19"},{sign:"Taurus",start:"04-20",end:"05-20"},{sign:"Gemini",start:"05-21",end:"06-20"},{sign:"Cancer",start:"06-21",end:"07-22"},{sign:"Leo",start:"07-23",end:"08-22"},{sign:"Virgo",start:"08-23",end:"09-22"},{sign:"Libra",start:"09-23",end:"10-22"},{sign:"Scorpio",start:"10-23",end:"11-21"},{sign:"Sagittarius",start:"11-22",end:"12-21"},{sign:"Capricorn",start:"12-22",end:"12-31"}];
const interpretations={Aries:"🔥 <strong>Aries</strong> – You're driven, bold, and always ready to pioneer new territory.",Taurus:"🌱 <strong>Taurus</strong> – Patience and practicality shape your world.",Gemini:"💨 <strong>Gemini</strong> – Curiosity is your compass.",Cancer:"🌊 <strong>Cancer</strong> – Feelings run deep.",Leo:"☀️ <strong>Leo</strong> – Creativity and confidence illuminate your path.",Virgo:"🌾 <strong>Virgo</strong> – Details matter.",Libra:"⚖️ <strong>Libra</strong> – Harmony seeker.",Scorpio:"🦂 <strong>Scorpio</strong> – Intense and transformative.",Sagittarius:"🏹 <strong>Sagittarius</strong> – Adventure calls.",Capricorn:"⛰️ <strong>Capricorn</strong> – Discipline and ambition are your allies.",Aquarius:"🌐 <strong>Aquarius</strong> – Innovator and humanitarian.",Pisces:"✨ <strong>Pisces</strong> – Intuitive and compassionate."};
const form=document.getElementById("birthForm"),resultDiv=document.getElementById("result");
function getSunSign(d){const mmdd=d.format("MM-DD");return(sunSignRanges.find(r=>mmdd>=r.start&&mmdd<=r.end)||{}).sign||"Unknown"}
form.addEventListener("submit",e=>{e.preventDefault();const dateVal=document.getElementById("birthdate").value;if(!dateVal){alert("Please enter your birth date");return;}const timeVal=document.getElementById("birthtime").value;
  const dateStr=timeVal?`${dateVal}T${timeVal}:00Z`:`${dateVal}T00:00:00Z`;
  const birthDate=dayjs(dateStr);const sun=getSunSign(birthDate);
  resultDiv.style.display="block";
  resultDiv.innerHTML=`<h3>📅 Birth data</h3><p>${birthDate.format("YYYY‑MM‑DD HH:mm")} (UTC)</p><h3>🪐 Natal chart</h3><div class='chart-placeholder'>SVG natal wheel coming soon</div><h3>📝 Interpretation</h3><div class='interp'>${interpretations[sun]||"Coming soon..."}</div>`;});
</script>
