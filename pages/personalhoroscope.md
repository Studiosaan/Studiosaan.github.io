---
layout: page
title: Personal Horoscope
permalink: /personalhoroscope/
subtitle: Discover your natal blueprint in a single page ✨
---

<!-- ── 스타일 (그대로 두거나 custom.scss로 빼도 OK) ── -->
<style>
:root{--primary:#3f51b5;--border:#e0e0e0;--radius:10px}
h2{text-align:center;font-weight:700;font-size:clamp(1.8rem,2.5vw + .5rem,2.4rem);margin:2rem 0 1rem}
.mx-auto{margin-inline:auto}.w-full{width:100%}
.card{max-width:480px;padding:1rem 1.25rem;border:1px solid var(--border);border-radius:var(--radius);background:#fff;box-shadow:0 2px 4px rgba(0,0,0,.05)}
form label{display:block;margin-bottom:.35rem;font-weight:600}
form input{width:100%;padding:.6rem .8rem;margin-bottom:1.1rem;border:1px solid #cfcfcf;border-radius:var(--radius);font-size:.95rem}
form button{width:100%;padding:.7rem;background:var(--primary);color:#fff;border:none;border-radius:var(--radius);font-size:.95rem;cursor:pointer;transition:background .2s ease}
form button:hover{background:#2d3aa0}
.result{max-width:700px;margin:2rem auto 3rem;padding:1.5rem 1.25rem;border:1px solid var(--border);border-radius:var(--radius);background:#fff;box-shadow:0 4px 6px rgba(0,0,0,.06);animation:fadeIn .4s ease}
#wheel svg{width:100%;height:auto}
@keyframes fadeIn{from{opacity:0;transform:translateY(10px)}to{opacity:1}}
@media(min-width:768px){form.card{display:flex;flex-wrap:wrap;gap:1rem;align-items:flex-end}form .field{flex:1 1 200px}form button{flex:1 1 150px;margin-bottom:0}}
</style>

<h2>🔍 Personal Horoscope</h2>

<form id="birthForm" class="card mx-auto">
  <div class="field w-full">
    <label for="birthdate">Birth date (YYYY-MM-DD)</label>
    <input type="date" id="birthdate" required>
  </div>
  <div class="field w-full">
    <label for="birthtime">Birth time (24h HH:MM) — optional</label>
    <input type="time" id="birthtime">
  </div>
  <button type="submit">Show my horoscope</button>
</form>

<div id="result" class="result" style="display:none;"></div>

<!-- ── 라이브러리 CDN ── -->
<script src="https://unpkg.com/dayjs@1/dayjs.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/astrochart@1.5.0/build/astrochart.min.js"></script>

<script>
/* 간단 Sun-Sign → 해석 텍스트 */
const interp={Leo:"☀️ Leo – Creativity and confidence illuminate your path.",…};

/* 폼 처리 */
document.getElementById('birthForm').addEventListener('submit',async e=>{
  e.preventDefault();
  const d   = document.getElementById('birthdate').value;
  if(!d){alert('enter date');return;}
  const t   = document.getElementById('birthtime').value || "00:00";
  const iso = `${d}T${t}:00Z`;
  const dt  = dayjs(iso);

  /* astrochart – 계산 + SVG 출력 */
  const wheelId ='wheel';
  document.getElementById('result').style.display='block';
  document.getElementById('result').innerHTML=`
    <h3>📅 Birth data</h3><p>${dt.format('YYYY-MM-DD HH:mm')} (UTC)</p>
    <h3>🪐 Natal chart</h3><div id="${wheelId}"></div>
    <h3>📝 Interpretation</h3><div>${interp['Leo']||''}</div>`;

  /* astrochart 라이브러리 호출 – 내부에서 Swiss Ephemeris WASM 자동 로드 */
  const chart = new Astral.Chart({
      id   : wheelId,
      utcDate: new Date(iso),
      // planetSet: 'all' 기본 (Sun ~ Pluto)
      width: 600
  });
  await chart.draw();             // SVG 삽입
});
</script>
