# index.html<!doctype html>
<html lang="de">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Selaconstruction Kennzahlen</title>
<style>
  :root { --bg:#ffffff; --fg:#111; --muted:#666; --card:#f7f7f7; }
  body{margin:0;background:var(--bg);font-family:system-ui,Arial,sans-serif}
  .wrap{max-width:1100px;margin:0 auto;padding:56px 20px}
  .grid{display:grid;grid-template-columns:repeat(3,minmax(0,1fr));gap:16px}
  .card{background:var(--card);border-radius:16px;padding:28px;text-align:center;
        box-shadow:0 6px 18px rgba(0,0,0,.06)}
  .label{font-size:13px;letter-spacing:.08em;text-transform:uppercase;color:var(--muted);margin-bottom:10px}
  .value{font-size:56px;font-weight:800;line-height:1}
  .unit{font-size:16px;color:var(--muted);margin-top:8px}
  @media (max-width:820px){ .grid{grid-template-columns:1fr;}}
</style>
</head>
<body>
  <div class="wrap" id="metrics">
    <div class="grid">
      <div class="card">
        <div class="label">Expertise</div>
        <div class="value" data-target="15">0</div>
        <div class="unit">Jahre schlüsselfertiges Bauen</div>
      </div>
      <div class="card">
        <div class="label">Projekte</div>
        <div class="value" data-target="42">0</div>
        <div class="unit">realisierte Bauvorhaben</div>
      </div>
      <div class="card">
        <div class="label">Apartments</div>
        <div class="value" data-target="24">0</div>
        <div class="unit">aktuell im Bau</div>
      </div>
    </div>
  </div>

<script>
(() => {
  const section = document.getElementById('metrics');
  const counters = [...section.querySelectorAll('.value')];
  let started = false;

  function animate(el, target, ms=1500){
    const start = performance.now();
    const step = (t) => {
      const p = Math.min((t - start)/ms, 1);
      el.textContent = Math.floor(p * target).toLocaleString('de-DE');
      if (p < 1) requestAnimationFrame(step);
      else el.textContent = target.toLocaleString('de-DE');
    };
    requestAnimationFrame(step);
  }

  const io = new IntersectionObserver((entries) => {
    entries.forEach(e=>{
      if (e.isIntersecting && !started){
        started = true;
        counters.forEach(el => animate(el, parseInt(el.dataset.target,10)));
        io.disconnect();
      }
    });
  }, { threshold: 0.35 });

  io.observe(section);
})();
</script>
</body>
</html>
