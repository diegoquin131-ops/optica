<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Lentes delgadas — Interactivo ligero</title>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;600&family=Press+Start+2P&display=swap" rel="stylesheet">
  <style>
    :root{--bg:#000000;--panel:#0f1216;--muted:#9aa6b2;--accent:#66f;--accent2:#6ff;--glass:rgba(255,255,255,0.03)}
    *{box-sizing:border-box}
    body{margin:0;font-family:Inter,system-ui,Arial;background:var(--bg);color:#eaf2ff}
    header{padding:18px 24px;border-bottom:1px solid rgba(255,255,255,0.03);display:flex;gap:16px;align-items:center}
    .logo{width:64px;height:64px;border-radius:10px;background:linear-gradient(135deg,#07122a,#001);display:flex;align-items:center;justify-content:center}
    .logo h1{font-family:'Press Start 2P',monospace;color:var(--accent);font-size:12px;margin:0}
    .title{font-size:18px;margin:0}
    nav{display:flex;gap:12px;margin-left:auto}
    nav button{background:transparent;border:1px solid var(--glass);padding:8px 10px;border-radius:8px;color:var(--muted);cursor:pointer;transition:background 0.2s, color 0.2s;}
    nav button:hover{background:rgba(255,255,255,0.08); color:#eaf2ff;}
    .wrap{max-width:1100px;margin:18px auto;padding:18px;display:grid;grid-template-columns:1fr 360px;gap:18px}
    .panel{background:linear-gradient(180deg, rgba(255,255,255,0.01), transparent);padding:14px;border-radius:12px;border:1px solid var(--glass)}
    .canvas-wrap{display:flex;flex-direction:column;gap:10px}
    canvas{width:100%;height:420px;border-radius:10px;background:linear-gradient(180deg,#021026,#001);display:block}
    .controls{display:flex;gap:8px;flex-wrap:wrap;align-items:center}
    label{font-size:13px;color:var(--muted)}
    select,input[type=range]{appearance:none;padding:6px;border-radius:8px;border:1px solid rgba(255,255,255,0.03);background:#08101a;color:var(--muted)}
    button.primary{background:linear-gradient(90deg,var(--accent),var(--accent2));border:0;color:#001;padding:8px 12px;border-radius:8px;cursor:pointer;transition:transform 0.1s ease;}
    button.primary:active{transform:scale(0.98);}
    .aside{display:flex;flex-direction:column;gap:12px}
    .thumb{height:180px;border-radius:10px;overflow:hidden;border:1px solid rgba(255,255,255,0.03);background:#071022}
    .thumb img{width:100%;height:100%;object-fit:cover}
    
    /* --- CSS para Pixel Art --- */
    .pixel-grid{display:grid;grid-template-columns:repeat(12,12px);grid-auto-rows:12px;gap:2px;padding:8px;background:#020217;border-radius:6px; margin-bottom: 5px; width: fit-content;}
    .px{width:12px;height:12px;border-radius:2px;opacity:.08}
    .px.on{opacity:1}
    .px.obj{background:var(--accent);box-shadow:0 0 6px rgba(102,102,255,0.35)}
    .px.img{background:var(--accent2);box-shadow:0 0 6px rgba(102,255,255,0.35)}
    .px.lens{background:#556;box-shadow:0 0 4px rgba(85,85,102,0.35)}

    /* --- CSS para Fórmulas --- */
    #formulas { background: #08101a; border: 1px solid var(--accent); }
    .formula-display{
      background:rgba(0,0,0,0.25);
      padding:16px;
      border-radius:8px;
      font-family:monospace;
      font-size:18px;
      text-align:center;
      color:#eaf2ff;
      margin-bottom:16px;
    }
    .formula-display strong{ color: var(--accent2); font-size: 20px; }
    
    .example-grid {
      margin-bottom: 16px; 
    }
    .example-grid p {
      margin: 0;
      font-size: 13px;
      color: var(--muted);
      line-height: 1.4;
    }

    /* --- CSS para Guía del Simulador --- */
    #explicacionSimulador, #conceptos-section {
        padding: 16px 20px;
    }
    .guia-subtitulo {
        font-size: 16px;
        color: var(--accent2);
        margin-top: 16px;
        margin-bottom: 10px;
        border-bottom: 1px solid var(--glass);
        padding-bottom: 6px;
    }
    .guia-lista {
        font-size: 13px;
        color: var(--muted);
        line-height: 1.6;
        padding-left: 20px;
    }
    .guia-lista li { margin-bottom: 8px; }
    .guia-lista code, .guia-lista strong {
        color: #eaf2ff; 
        font-weight: 600;
    }
    .rayo-naranja { color: rgba(255, 200, 80, 0.95); }
    .rayo-verde { color: rgba(80, 255, 200, 0.95); }
    .rayo-magenta { color: rgba(255, 100, 255, 0.95); }

    /* --- Estilos para la nueva sección de Conceptos --- */
    #conceptos .concepto-titulo {
        font-size: 16px;
        color: var(--accent); /* Un color que destaque */
        margin-top: 16px;
        margin-bottom: 8px;
        border-bottom: 1px solid var(--glass);
        padding-bottom: 4px;
    }
    #conceptos p {
        font-size: 13px;
        color: var(--muted);
        line-height: 1.6;
        margin-bottom: 12px;
    }
    #conceptos p:last-of-type {
        margin-bottom: 0;
    }

    footer{text-align:center;color:var(--muted);margin-top:12px}
    @media (max-width:1040px){.wrap{grid-template-columns:1fr}canvas{height:300px}}
  </style>
</head>
<body>
  <header>
    <div class="logo"><h1>LENTES</h1></div>
    <div>
      <div class="title">Lentes delgadas — Interactivo ligero</div>
      <div style="font-size:13px;color:var(--muted)">Simulador optimizado — mueve controles o anima con espacio</div>
    </div>
    <nav>
      <button id="btnIntro">Inicio</button>
      <button id="btnConceptos">Conceptos</button>
      <button id="btnTipos">Tipos</button>
      <button id="btnForm">Fórmulas</button>
    </nav>
  </header>

  <main class="wrap">
    <section class="panel canvas-wrap" aria-labelledby="sim-title">
      <h3 id="sim-title">Simulador de rayos (Optimizado y Corregido)</h3>
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
          <p style="color:var(--muted);font-size:13px;margin:6px 0 0">Usa los controles para cambiar la lente y las distancias. Presiona espacio para iniciar/detener la animación.</p>
        </div>
      </div>

      <div class="panel" id="explicacionSimulador">
        <h4 class="guia-subtitulo" style="margin-top: 0;">Guía de Elementos del Simulador</h4>
        <ul class="guia-lista">
          <li><strong>Flecha Blanca (Objeto):</strong> Representa el objeto real que estás observando. Su posición es la <code>d_o</code> (distancia objeto).</li>
          <li><strong>Flecha Azul/Cian (Imagen):</strong> Es la imagen formada por la lente. Si es azul sólida, es una imagen <strong>Real</strong>. Si es cian y punteada, es <strong>Virtual</strong>.</li>
          <li><strong>Línea Central Horizontal:</strong> Es el <strong>Eje Óptico</strong>, la línea de simetría.</li>
          <li><strong>Forma en el Centro (Lente):</strong> Representa la lente delgada (Convexa o Cóncava).</li>
          <li><strong>Puntos (F) y (F'):</strong> Son los <strong>Puntos Focales</strong>. Su distancia al centro es <code>f</code>.</li>
          <li class="rayo-naranja"><strong>Línea Naranja:</strong> <strong>Rayo Paralelo</strong>. Sale paralelo al eje, cruza la lente y se desvía hacia el foco <code>F'</code>.</li>
          <li class="rayo-verde"><strong>Línea Verde:</strong> <strong>Rayo Central</strong>. Pasa por el centro de la lente y no se desvía.</li>
          <li class="rayo-magenta"><strong>Línea Magenta:</strong> <strong>Rayo Focal</strong>. Pasa por el foco <code>F</code>, cruza la lente y sale paralelo.</li>
        </ul>

        <h4 class="guia-subtitulo">Guía de Controles (Etiquetas)</h4>
        <ul class="guia-lista">
          <li><strong>Tipo:</strong> Cambia entre lente <strong>Convergente</strong> (positiva) y <strong>Divergente</strong> (negativa).</li>
          <li><strong>Slider F (px):</strong> Controla la <strong>Distancia Focal (<code>f</code>)</strong>.</li>
          <li><strong>Slider d_o (px):</strong> Controla la <strong>Distancia del Objeto (<code>d_o</code>)</strong>.</li>
          <li><strong>Resultados:</strong> Muestra la <strong>Distancia de la Imagen (<code>d_i</code>)</strong> y el <strong>Aumento (<code>A</code>)</strong> calculados.</li>
        </ul>
      </div>

    </section>

    <aside class="aside">

      <div class="panel" id="conceptos">
        <h4>Conceptos Ópticos</h4>
        <h5 class="concepto-titulo">Superficies Reflectantes</h5>
        <p>
          Es cualquier superficie que devuelve la luz que incide sobre ella. Este fenómeno se llama reflexión. Puede ser <strong>especular</strong> (cuando la luz se refleja en una dirección definida, como en un espejo pulido) o <strong>difusa</strong> (cuando la luz se dispersa en muchas direcciones, como en una pared mate).
        </p>
        
        <h5 class="concepto-titulo">Espejos Esféricos</h5>
        <p>
          Son superficies reflectantes que poseen la forma de una sección de una esfera. Se clasifican principalmente en:
        </p>
        <ul class="guia-lista" style="padding-left: 15px;">
          <li><strong>Espejos Cóncavos:</strong> La superficie reflectante es la parte interna de la esfera. Tienden a <strong>converger</strong> los rayos de luz paralelos en un punto focal.</li>
          <li><strong>Espejos Convexos:</strong> La superficie reflectante es la parte externa de la esfera. Tienden a <strong>divergir</strong> los rayos de luz, haciendo que parezcan provenir de un punto focal detrás del espejo.</li>
        </ul>
      </div>

      <div class="panel" id="tipos">
        <h4>Tipos de lentes</h4>
        <div class="thumb">
          
`
Aquí tienes una representación visual de distintos tipos de lentes, destacando sus formas características:
`
<img src="https://i.ibb.co/L5r03w1/a4af1a2d54ac35da1c982f0b8ca390506bfeb7c8.webp" alt="Tipos de lentes">
        </div>
        <p style="color:var(--muted);font-size:13px;margin:6px 0 0">Las lentes delgadas se clasifican en convergentes y divergentes. Las convergentes enfocan rayos paralelos; las divergentes los dispersan.</p>
      </div>

      <div class="panel" id="formulas">
        <h4>Fórmula y Ejemplos</h4>
        
        <div class="formula-display">
          1/<strong>f</strong> = 1/<strong>d<sub>o</sub></strong> + 1/<strong>d<sub>i</sub></strong>
        </div>
        
        <strong style="font-size: 14px; color: var(--muted); margin-left: 4px; display: block; margin-bottom: 8px;">Casos Principales:</strong>
        
        <div class="example-grid">
          <div class="pixel-grid" id="pixelEx1"></div>
          <p><strong>Conv. (d<sub>o</sub> > f): Real, Invertida</strong><br>
            El objeto está fuera del foco. Produce una imagen real (como un proyector).</p>
        </div>
        
        <div class="example-grid">
          <div class="pixel-grid" id="pixelEx2"></div>
          <p><strong>Conv. (d<sub>o</sub> < f): Virtual, Derecha</strong><br>
            El objeto está dentro del foco. Produce una imagen virtual ampliada (como una lupa).</p>
        </div>
        
        <div class="example-grid">
          <div class="pixel-grid" id="pixelEx3"></div>
          <p><strong>Divergente: Virtual, Derecha, Menor</strong><br>
            Siempre produce una imagen virtual, derecha y más pequeña (como un lente de miopía).</p>
        </div>

        <strong style="font-size: 14px; color: var(--muted); margin-left: 4px; display: block; margin-top: 20px; border-top: 1px solid var(--glass); padding-top: 16px;">
          Leyenda de Colores (Ejemplos)
        </strong>
        <p style="font-size: 13px; color: var(--muted); margin: 10px 0 0 4px; line-height: 1.6;">
          🟦 <strong>Cuadrados Azules:</strong> Representan el <strong>Objeto</strong>.<br>
          🌐 <strong>Cuadrados Cian:</strong> Representan la <strong>Imagen</strong>.<br>
          🗄️ <strong>Cuadrados Grises:</strong> Representan la <strong>Lente</strong>.
        </p>
        </div>
    </aside>
  </main>

  <footer style="padding:12px">Simulador optimizado para bajo consumo de recursos.</footer>

  <script>
    // --- Construcción pixel-art para ejemplos ---
    (function(){
      const ex1_arr = [0,0,1,0,0,3,0,0,0,2,0,0,0,0,1,0,0,3,0,0,0,2,0,0,0,0,1,0,0,3,0,0,0,0,0,0,0,0,0,0,0,3,0,0,0,2,0,0,0,0,0,0,0,3,0,0,0,2,0,0];
      const ex2_arr = [0,2,2,0,1,0,3,0,0,0,0,0,0,2,2,0,1,0,3,0,0,0,0,0,0,2,2,0,1,0,3,0,0,0,0,0,0,2,2,0,0,0,3,0,0,0,0,0,0,2,2,0,0,0,3,0,0,0,0,0];
      const ex3_arr = [0,0,1,0,0,3,0,3,0,0,0,0,0,0,1,0,2,0,3,0,0,0,0,0,0,0,1,0,2,0,3,0,0,0,0,0,0,0,1,0,0,3,0,3,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0];

      function build(id,arr){
        const el=document.getElementById(id);
        arr.forEach(v=>{
          const d=document.createElement('div');
          d.className='px';
          if(v === 1) d.classList.add('obj');
          if(v === 2) d.classList.add('img');
          if(v === 3) d.classList.add('lens');
          if(v > 0) d.classList.add('on');
          el.appendChild(d);
        });
      }
      build('pixelEx1', ex1_arr);
      build('pixelEx2', ex2_arr);
      build('pixelEx3', ex3_arr);
    })();

    // --- Simulador ligero (OPTIMIZADO Y CORREGIDO) ---
    const canvas=document.getElementById('scene');
    const ctx=canvas.getContext('2d');
    
    function debounce(func, wait) {
      let timeout;
      return function(...args) {
        const context = this;
        clearTimeout(timeout);
        timeout = setTimeout(() => func.apply(context, args), wait);
      };
    }

    function fitCanvas(){
      const dpr=window.devicePixelRatio||1;
      canvas.width=canvas.clientWidth*dpr;
      canvas.height=canvas.clientHeight*dpr;
      ctx.setTransform(dpr,0,0,dpr,0,0);
      lastState = {}; 
    }   
    window.addEventListener('resize', debounce(fitCanvas, 100));

    const lensType=document.getElementById('lensType');
    const fRange=document.getElementById('fRange');
    const doRange=document.getElementById('doRange');
    const resText=document.getElementById('resText');
    const showLabels=document.getElementById('showLabels');
    const toggleAnim=document.getElementById('toggleAnim');
    const resetBtn=document.getElementById('resetBtn');

    let anim = false;
    let animT = 0;
    let lastState = {}; 

    function clear(){ ctx.clearRect(0,0,canvas.width,canvas.height); }
    function computeDi(f, d_o){ const denom=(1/f - 1/d_o); if(Math.abs(denom)<1e-6) return Infinity; return 1/denom; }

    function drawLens(x,type){
      ctx.save();
      ctx.translate(x, canvas.clientHeight/2);
      ctx.fillStyle = 'rgba(120, 200, 255, 0.08)';
      ctx.strokeStyle = 'rgba(120, 200, 255, 0.4)';
      ctx.lineWidth = 1.5;
      ctx.beginPath();
      if(type==='convergent'){
        ctx.moveTo(0, -100); ctx.quadraticCurveTo(30, 0, 0, 100); ctx.quadraticCurveTo(-30, 0, 0, -100);
      } else {
        ctx.moveTo(10, -100); ctx.quadraticCurveTo(-20, 0, 10, 100); ctx.moveTo(-10, -100); ctx.quadraticCurveTo(20, 0, -10, 100); ctx.lineTo(10, 100);
      }
      ctx.fill(); ctx.stroke(); ctx.restore();
    }
    
    function drawRay(p1, p2, isVirtual) {
        ctx.beginPath(); ctx.moveTo(p1.x, p1.y);
        ctx.setLineDash(isVirtual ? [4, 4] : []);
        ctx.lineTo(p2.x, p2.y); ctx.stroke();
    }

    function draw(state){
      clear();
      const cw=canvas.clientWidth, ch=canvas.clientHeight;
      const lensX=cw/2; const axisY = ch/2;
      const { type, f: fRaw, d_o, labels } = state;
      const signedF = (type==='convergent')? fRaw : -fRaw;
      const d_i = computeDi(signedF, d_o);
      
      ctx.strokeStyle='rgba(255,255,255,0.06)'; ctx.lineWidth=1; ctx.setLineDash([]);
      ctx.beginPath(); ctx.moveTo(0,axisY); ctx.lineTo(cw,axisY); ctx.stroke();
      drawLens(lensX,type);
      
      const objX = lensX - d_o; const objH = 60; const objTop = axisY - objH;
      ctx.strokeStyle='white'; ctx.lineWidth=2; ctx.setLineDash([]);
      ctx.beginPath(); ctx.moveTo(objX,axisY); ctx.lineTo(objX,objTop); ctx.stroke();
      ctx.beginPath(); ctx.moveTo(objX-6,objTop+6); ctx.lineTo(objX,objTop); ctx.lineTo(objX+6,objTop+6); ctx.fillStyle='white'; ctx.fill();
      if(labels){ ctx.fillStyle='rgba(255,255,255,0.8)'; ctx.font='13px Inter'; ctx.fillText('Objeto (o)', objX - 15, objTop-8); }

      const f1x = lensX - signedF; const f2x = lensX + signedF;
      if(labels){
          ctx.fillStyle = 'rgba(255, 100, 100, 0.8)'; ctx.font = '12px Inter';
          ctx.fillText('F', f1x - 5, axisY + 18); ctx.fillText("F'", f2x - 5, axisY + 18);
      }

      ctx.lineWidth=1.6;
      const objP = {x: objX, y: objTop}; 

      ctx.strokeStyle='rgba(255, 200, 80, 0.95)';
      const p1_lens = {x: lensX, y: objTop};
      const slope1 = (axisY - objTop) / (f2x - lensX);
      const p1_end = {x: cw, y: objTop + slope1 * (cw - lensX)};
      drawRay(objP, p1_lens, false); drawRay(p1_lens, p1_end, false);
      if (type === 'divergent') { drawRay(p1_lens, {x: f2x, y: axisY}, true); }

      ctx.strokeStyle='rgba(80, 255, 200, 0.95)';
      const slope2 = (objTop - axisY) / (objX - lensX);
      const p2_end = {x: cw, y: axisY + slope2 * (cw - lensX)};
      const p2_start = {x: 0, y: axisY + slope2 * (0 - lensX)};
      drawRay(objP, p2_end, false); drawRay(objP, p2_start, true);

      ctx.strokeStyle='rgba(255, 100, 255, 0.95)';
      const slope3 = (objTop - axisY) / (objX - f1x);
      const p3_lens = {x: lensX, y: axisY + slope3 * (lensX - f1x)};
      const p3_end = {x: cw, y: p3_lens.y};
      drawRay(objP, p3_lens, false); drawRay(p3_lens, p3_end, false);
s      if (type === 'convergent' && (objX - f1x) !== 0) { drawRay(objP, {x: f1x, y: axisY}, true); }

      if(isFinite(d_i)){
        const imgX = lensX + d_i; const A = -d_i/d_o; const imgTop = axisY - (objH * A); const isVirtual = d_i < 0;
        ctx.strokeStyle = isVirtual ? 'rgba(120,200,255,0.5)' : 'rgba(120,200,255,0.95)';
        ctx.fillStyle = isVirtual ? 'rgba(120,200,255,0.5)' : 'rgba(120,200,255,0.95)';
        ctx.lineWidth=2;
        drawRay({x: imgX, y: axisY}, {x: imgX, y: imgTop}, isVirtual);
        ctx.setLineDash([]); ctx.beginPath();
        const arrowDir = (A > 0) ? 6 : -6;
        ctx.moveTo(imgX - 6, imgTop + arrowDir); ctx.lineTo(imgX, imgTop); ctx.lineTo(imgX + 6, imgTop + arrowDir);
        ctx.fill();
        if(labels) ctx.fillText('Imagen (i)', imgX + 8, imgTop + (A > 0 ? -6 : 6) );
        resText.textContent = `d_i = ${d_i.toFixed(1)} px — ${isVirtual? 'virtual' : 'real'} · A=${A.toFixed(2)}`;
      } else {
        resText.textContent = 'd_i → ∞ (imagen en el infinito)';
      }
    }

    function getCurrentState() {
        return {
            type: lensType.value,
            f: parseFloat(fRange.value),
            d_o: parseFloat(doRange.value),
            labels: showLabels.checked
        };
    }
    
    function renderLoop(){
      if(anim){
        animT += 0.008; 
        const base = 380; const range = 320;
        doRange.value = base + Math.sin(animT) * range;
      }
      
      const newState = getCurrentState();
      
      if (newState.type !== lastState.type ||
          newState.f !== lastState.f ||
          newState.d_o !== lastState.d_o ||
          newState.labels !== lastState.labels)
      {
          draw(newState);
          lastState = newState; 
      }
      
      requestAnimationFrame(renderLoop); 
    }

    lensType.addEventListener('change', () => { lastState = {}; });
    showLabels.addEventListener('change', () => { lastState = {}; });

    toggleAnim.addEventListener('click', ()=>{
      anim = !anim;
      toggleAnim.textContent = anim? 'Parar animación' : 'Iniciar animación';
    });
    
    resetBtn.addEventListener('click', ()=>{
      fRange.value=140; doRange.value=320; lensType.value='convergent';
      showLabels.checked = true; anim = false;
      toggleAnim.textContent = 'Iniciar animación';
      lastState = {}; 
    });

    window.addEventListener('keydown',(e)=>{
      if(e.code==='Space'){
        e.preventDefault(); anim=!anim;
        toggleAnim.textContent = anim? 'Parar animación' : 'Iniciar animación';
      }
    });

    document.getElementById('btnIntro').addEventListener('click', ()=>{ window.scrollTo({top:0,behavior:'smooth'}); });
    document.getElementById('btnConceptos').addEventListener('click', ()=>{ document.getElementById('conceptos').scrollIntoView({behavior:'smooth'}); });
    document.getElementById('btnTipos').addEventListener('click', ()=>{ document.getElementById('tipos').scrollIntoView({behavior:'smooth'}); });
    document.getElementById('btnForm').addEventListener('click', ()=>{ document.getElementById('formulas').scrollIntoView({behavior:'smooth'}); });

    fitCanvas();
    renderLoop(); 

  </script>
</body>
</html>
