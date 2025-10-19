<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Lentes delgadas — Interactivo ligero</title>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;600&family=Press+Start+2P&display=swap" rel="stylesheet">
  <style>
    :root{--bg:#0b0b0b;--panel:#0f1216;--muted:#9aa6b2;--accent:#66f;--accent2:#6ff;--glass:rgba(255,255,255,0.03)}
    *{box-sizing:border-box}
    body{margin:0;font-family:Inter,system-ui,Arial;background:var(--bg);color:#eaf2ff}
    header{padding:18px 24px;border-bottom:1px solid rgba(255,255,255,0.03);display:flex;gap:16px;align-items:center}
    .logo{width:64px;height:64px;border-radius:10px;background:linear-gradient(135deg,#07122a,#001);display:flex;align-items:center;justify-content:center}
    .logo h1{font-family:'Press Start 2P',monospace;color:var(--accent);font-size:12px;margin:0}
    .title{font-size:18px;margin:0}
    nav{display:flex;gap:12px;margin-left:auto}
    nav button{background:transparent;border:1px solid var(--glass);padding:8px 10px;border-radius:8px;color:var(--muted);cursor:pointer}

    .wrap{max-width:1100px;margin:18px auto;padding:18px;display:grid;grid-template-columns:1fr 360px;gap:18px}

    .panel{background:linear-gradient(180deg, rgba(255,255,255,0.01), transparent);padding:14px;border-radius:12px;border:1px solid var(--glass)}
    .canvas-wrap{display:flex;flex-direction:column;gap:10px}
    canvas{width:100%;height:420px;border-radius:10px;background:linear-gradient(180deg,#021026,#001);display:block}

    .controls{display:flex;gap:8px;flex-wrap:wrap;align-items:center}
    label{font-size:13px;color:var(--muted)}
    select,input[type=range]{appearance:none;padding:6px;border-radius:8px;border:1px solid rgba(255,255,255,0.03);background:#08101a;color:var(--muted)}
    button.primary{background:linear-gradient(90deg,var(--accent),var(--accent2));border:0;color:#001;padding:8px 12px;border-radius:8px;cursor:pointer}

    .aside{display:flex;flex-direction:column;gap:12px}
    .thumb{height:180px;border-radius:10px;overflow:hidden;border:1px solid rgba(255,255,255,0.03);background:#071022}
    .thumb img{width:100%;height:100%;object-fit:cover}

    .pixel-row{display:flex;gap:8px;align-items:center}
    .pixel-grid{display:grid;grid-template-columns:repeat(12,12px);grid-auto-rows:12px;gap:2px;padding:8px;background:#020217;border-radius:6px}
    .px{width:12px;height:12px;border-radius:2px;opacity:.08}
    .px.on{opacity:1;background:var(--accent);box-shadow:0 0 6px rgba(102,102,255,0.35)}

    pre{background:rgba(0,0,0,0.25);padding:10px;border-radius:8px;overflow:auto}
    footer{text-align:center;color:var(--muted);margin-top:12px}

    @media (max-width:1040px){.wrap{grid-template-columns:1fr}canvas{height:300px}}
  </style>
</head>
<body>
  <header>
    <div class="logo"><h1>LENTES</h1></div>
    <div>
      <div class="title">Lentes delgadas — Interactivo ligero</div>
      <div style="font-size:13px;color:var(--muted)">Simulador optimizado para no trabarse — mueve controles o anima con espacio</div>
    </div>
    <nav>
      <button id="btnIntro">Inicio</button>
      <button id="btnTipos">Tipos</button>
      <button id="btnForm">Fórmulas</button>
    </nav>
<script>
// Botón para activar/desactivar simulación
let animando = false;
function toggleSimulacion(){
  animando = !animando;
  document.getElementById('btnSim').innerText = animando ? 'Detener Simulación' : 'Iniciar Simulación';
}
</script>
  </header>

  <main class="wrap">
    <section class="panel canvas-wrap" aria-labelledby="sim-title">
      <h3 id="sim-title">Simulador de rayos (ligero)</h3>
      <canvas id="scene" width="900" height="420" aria-label="Simulador de lentes"></canvas>

      <div class="controls">
        <label>Tipo:
          <select id="lensType">
            <option value="convergent">Convergente (convexa)</option>
            <option value="divergent">Divergente (cóncava)</option>
          </select>
        </label>

        <label>F (px): <input id="fRange" type="range" min="60" max="300" value="140"></label>
        <label>d_o (px): <input id="doRange" type="range" min="60" max="700" value="320"></label>

        <label><input id="showLabels" type="checkbox" checked> Mostrar etiquetas</label>
        <button id="toggleAnim" class="primary">Iniciar animación</button>
        <button id="resetBtn">Reset</button>
      </div>

      <div style="display:flex;gap:12px;flex-wrap:wrap;margin-top:8px">
        <div style="flex:1" class="panel" id="infoPanel">Resultados: <span id="resText">—</span></div>
        <div style="width:260px" class="panel">
          <strong>Instrucciones</strong>
          <p style="color:var(--muted);font-size:13px;margin:6px 0 0">Usa los controles para cambiar la lente y las distancias. Presiona espacio para iniciar/detener la animación. El simulador usa aproximación de lente delgada.</p>
        </div>
      </div>
    </section>

    <aside class="aside">
      <div class="panel" id="tipos">
        <h4>Tipos de lentes<br><img src="a4af1a2d54ac35da1c982f0b8ca390506bfeb7c8.webp" alt="Tipos de lentes" style="width:60%;display:block;margin:10px auto;border:2px solid #00aaff;border-radius:10px;clip-path:inset(0 0 20% 0);"></h4>
        <div class="thumb"><img src="/mnt/data/a4af1a2d54ac35da1c982f0b8ca390506bfeb7c8.webp" alt="Tipos de lentes"></div>
        <p style="color:var(--muted);font-size:13px;margin:6px 0 0">Las lentes delgadas se clasifican en convergentes y divergentes. Las convergentes enfocan rayos paralelos a un punto; las divergentes los dispersan.</p>
      </div>

      <div class="panel">
        <h4>Pixel-art</h4>
        <div class="pixel-row">
          <div class="pixel-grid" id="pixelConv"></div>
          <div class="pixel-grid" id="pixelDiv"></div>
        </div>
        <div style="color:var(--muted);font-size:13px;margin-top:8px">Conv — centro grueso · Div — centro delgado</div>
      </div>

      <div class="panel" id="formulas">
        <h4>Fórmulas y ejemplos</h4>
        <pre>
Ecuación de lentes delgadas:
1/f = 1/d_o + 1/d_i

Aumento:
A = - d_i / d_o

Signos: f&gt;0 (convergente), f&lt;0 (divergente)

Ejemplos:
- d_o &gt; f (convergente): imagen real, invertida.
- d_o &lt; f (convergente): imagen virtual, derecha, ampliada.
- divergente: siempre imagen virtual, derecha, menor.
        </pre>
      </div>
    </aside>

  </main>

  <footer style="padding:12px">Optimizado para ser ligero — si tu navegador se queda sin memoria, cierra otras pestañas. Presiona espacio para animar.</footer>

  <script>
    // --- Construcción pixel-art pequeña y eficiente ---
    (function(){
      const conv=[0,0,0,0,1,1,1,1,0,0,0,0,0,1,1,1,1,1,1,1,0,0,0,0,1,1,1,1,1,1,1,1,1,1,1,0,0,1,1,1,1,1,1,1,1,0,0,0,1,1,1,1,1,1,1,1,0,0,0,0];
      const div=[0,0,1,1,0,0,0,0,1,1,0,0,0,1,1,1,1,1,1,1,1,1,0,0,1,1,1,1,1,1,1,1,1,1,1,0,0,1,1,1,1,1,1,1,1,0,0,0,1,1,1,1,1,0,0,0,0];
      function build(id,arr){const el=document.getElementById(id);arr.forEach(v=>{const d=document.createElement('div');d.className='px';if(v) d.classList.add('on');el.appendChild(d)});}
      build('pixelConv',conv);build('pixelDiv',div);
    })();

    // --- Simulador ligero ---
    const canvas=document.getElementById('scene');
    const ctx=canvas.getContext('2d');
    // use devicePixelRatio for crispness while keeping layout size
    function fitCanvas(){const dpr=window.devicePixelRatio||1;canvas.width=canvas.clientWidth*dpr;canvas.height=canvas.clientHeight*dpr;ctx.setTransform(dpr,0,0,dpr,0,0);}    
    fitCanvas();window.addEventListener('resize', ()=>{fitCanvas(); draw();});

    const lensType=document.getElementById('lensType');
    const fRange=document.getElementById('fRange');
    const doRange=document.getElementById('doRange');
    const resText=document.getElementById('resText');
    const showLabels=document.getElementById('showLabels');
    const toggleAnim=document.getElementById('toggleAnim');
    const resetBtn=document.getElementById('resetBtn');

    let anim=false; let animT=0;

    function clear(){ctx.clearRect(0,0,canvas.width,canvas.height);}

    // thin-lens calculation with units in px; sign convention: f>0 convergent, f<0 divergent
    function computeDi(f, d_o){ const denom=(1/f - 1/d_o); if(Math.abs(denom)<1e-6) return Infinity; return 1/denom; }

    function drawLens(x,type){ ctx.save(); ctx.translate(x,canvas.clientHeight/2); ctx.fillStyle='rgba(255,255,255,0.02)'; ctx.beginPath();
      if(type==='convergent'){ ctx.ellipse(0,0,20,100,0,0,Math.PI*2);} else { ctx.ellipse(0,0,20,100,0,0,Math.PI*2);} ctx.fill(); ctx.restore();
    }

    // draw once per frame but minimal operations
    function draw(){ clear(); const cw=canvas.clientWidth, ch=canvas.clientHeight; const lensX=cw/2; const type=lensType.value; const fRaw=parseFloat(fRange.value); const d_o=parseFloat(doRange.value);
      const signedF = (type==='convergent')? fRaw : -fRaw; const d_i = computeDi(signedF, d_o);
      // draw axis
      ctx.strokeStyle='rgba(255,255,255,0.06)'; ctx.lineWidth=1; ctx.beginPath(); ctx.moveTo(0,ch/2); ctx.lineTo(cw,ch/2); ctx.stroke();
      // lens
      drawLens(lensX,type);
      // object arrow
      const objX = lensX - d_o; const objTop = ch/2 - 60;
      ctx.strokeStyle='white'; ctx.lineWidth=2; ctx.beginPath(); ctx.moveTo(objX,ch/2); ctx.lineTo(objX,objTop); ctx.stroke(); ctx.beginPath(); ctx.moveTo(objX-8,objTop+8); ctx.lineTo(objX+8,objTop+8); ctx.lineTo(objX,objTop); ctx.fillStyle='white'; ctx.fill();
      if(showLabels.checked){ ctx.fillStyle='rgba(255,255,255,0.8)'; ctx.font='13px Inter'; ctx.fillText('Objeto (d_o)', Math.max(8, objX-80), objTop-8); }

      // rays (3 main rays) — compute minimal points to draw
      const rays = [];
      const y0 = objTop;
      // ray 1: parallel -> through focal
      if(type==='convergent'){
        const fx = lensX + signedF; // focal point on right
        rays.push([{x:objX,y:y0},{x:lensX,y:y0},{x:cw,y:y0 + (0 - y0)*(cw-lensX)/(fx-lensX)}]);
      } else {
        // diverging: make it look like it refracts as if from left focal
        const virtualFx = lensX - Math.abs(signedF);
        rays.push([{x:objX,y:y0},{x:lensX,y:y0},{x:cw,y:y0}]);
      }
      // ray 2: through center (straight)
      rays.push([{x:objX,y:y0},{x:cw,y:y0}]);
      // ray 3: toward focal on object side -> exits parallel
      rays.push([{x:objX,y:y0},{x:lensX,y:y0},{x:cw,y:y0}]);

      ctx.lineWidth=1.6; ctx.strokeStyle='rgba(255,200,80,0.95)';
      rays.forEach(r=>{ ctx.beginPath(); ctx.moveTo(r[0].x, r[0].y); for(let i=1;i<r.length;i++) ctx.lineTo(r[i].x, r[i].y); ctx.stroke(); });

      // image drawing
      if(isFinite(d_i)){
        const imgX = lensX + d_i; const A = -d_i/d_o; const imgHeight = ch/2 - (60 * A);
        ctx.strokeStyle='rgba(120,200,255,0.95)'; ctx.lineWidth=2; ctx.beginPath(); ctx.moveTo(imgX,ch/2); ctx.lineTo(imgX,imgHeight); ctx.stroke(); ctx.beginPath(); ctx.moveTo(imgX-8,imgHeight+8); ctx.lineTo(imgX+8,imgHeight+8); ctx.lineTo(imgX,imgHeight); ctx.fillStyle='rgba(120,200,255,0.95)'; ctx.fill(); if(showLabels.checked) ctx.fillText('Imagen (d_i)', Math.min(cw-140, imgX+8), imgHeight-6);
        resText.textContent = `d_i = ${d_i.toFixed(1)} px — ${d_i>0? 'real' : 'virtual'} · A=${A.toFixed(2)}`;
      } else { resText.textContent = 'd_i → ∞ (imagen en el infinito)'; }
    }

    // animation loop — only update controls when animating
    function loop(){ if(anim){ animT += 0.02; const base=320; const range=80; doRange.value = Math.max(60, Math.min(700, base + Math.sin(animT)*range)); draw(); } requestAnimationFrame(loop); }
    loop();

    // event handlers lightweight
    [lensType,fRange,doRange,showLabels].forEach(el=>el.addEventListener('input', ()=>{ draw(); }))

    toggleAnim.addEventListener('click', ()=>{ anim = !anim; toggleAnim.textContent = anim? 'Parar animación' : 'Iniciar animación'; });
    resetBtn.addEventListener('click', ()=>{ fRange.value=140; doRange.value=320; lensType.value='convergent'; draw(); });

    // keyboard space toggles anim
    window.addEventListener('keydown',(e)=>{ if(e.code==='Space'){ e.preventDefault(); anim=!anim; toggleAnim.textContent = anim? 'Parar animación' : 'Iniciar animación'; } });

    // initial draw
    draw();

    // navigation buttons (scroll to panels)
    document.getElementById('btnIntro').addEventListener('click', ()=>{ window.scrollTo({top:0,behavior:'smooth'}); });
    document.getElementById('btnTipos').addEventListener('click', ()=>{ document.getElementById('tipos').scrollIntoView({behavior:'smooth'}); });
    document.getElementById('btnForm').addEventListener('click', ()=>{ document.getElementById('formulas').scrollIntoView({behavior:'smooth'}); });
  </script>
</body>
</html>
