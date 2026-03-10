---
layout: single
title: "Molecule Viewer"
permalink: /molviewer/
author_profile: true
---

<style>
@import url('https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;600&family=Syne:wght@400;600;700&display=swap');
.mv-wrap{font-family:'Syne',sans-serif;background:#0d1117;border-radius:16px;padding:28px;color:#e6edf3;margin:0 -12px;box-shadow:0 0 60px rgba(56,189,248,0.07)}
.mv-title{font-size:1.3rem;font-weight:700;color:#38bdf8;letter-spacing:.04em;margin-bottom:6px}
.mv-subtitle{font-size:.82rem;color:#8b949e;margin-bottom:22px;font-family:'JetBrains Mono',monospace}
.mv-textarea{width:100%;min-height:130px;background:#161b22;border:1.5px solid #30363d;border-radius:10px;color:#c9d1d9;font-family:'JetBrains Mono',monospace;font-size:.78rem;padding:12px;resize:vertical;outline:none;transition:border .2s;box-sizing:border-box}
.mv-textarea:focus{border-color:#38bdf8}
.mv-select{background:#161b22;border:1.5px solid #30363d;border-radius:8px;color:#c9d1d9;font-family:'Syne',sans-serif;font-size:.82rem;padding:8px 12px;outline:none;cursor:pointer;transition:border .2s;flex:1;min-width:130px}
.mv-select:focus{border-color:#38bdf8}
.mv-label{font-size:.75rem;color:#8b949e;margin-bottom:5px;font-family:'JetBrains Mono',monospace;letter-spacing:.05em}
.mv-field{flex:1;min-width:130px}
.mv-btn{padding:10px 22px;border-radius:8px;border:none;font-family:'Syne',sans-serif;font-weight:600;font-size:.85rem;cursor:pointer;transition:all .2s;letter-spacing:.03em}
.mv-btn-primary{background:linear-gradient(135deg,#38bdf8,#0ea5e9);color:#0d1117}
.mv-btn-primary:hover{transform:translateY(-1px);box-shadow:0 4px 20px rgba(56,189,248,.35)}
.mv-btn-secondary{background:#21262d;color:#c9d1d9;border:1.5px solid #30363d}
.mv-btn-secondary:hover{border-color:#38bdf8;color:#38bdf8}
.mv-btn-danger{background:#21262d;color:#f85149;border:1.5px solid #30363d}
.mv-viewer-box{width:100%;height:460px;background:#010409;border-radius:12px;border:1.5px solid #21262d;position:relative;overflow:hidden;margin:18px 0}
.mv-placeholder{position:absolute;inset:0;display:flex;flex-direction:column;align-items:center;justify-content:center;color:#30363d;font-family:'JetBrains Mono',monospace;font-size:.85rem;text-align:center;gap:14px}
.mv-placeholder-icon{font-size:3rem;opacity:.4}
.mv-controls-row{display:flex;gap:10px;flex-wrap:wrap;align-items:flex-end;margin-bottom:14px}
.mv-toggle{display:flex;align-items:center;gap:7px;font-size:.8rem;color:#8b949e;cursor:pointer;padding:8px 14px;background:#161b22;border:1.5px solid #30363d;border-radius:8px;transition:all .2s;user-select:none}
.mv-toggle:hover{border-color:#38bdf8;color:#38bdf8}
.mv-toggle.active{border-color:#38bdf8;color:#38bdf8;background:rgba(56,189,248,.08)}
.mv-status{font-family:'JetBrains Mono',monospace;font-size:.75rem;padding:10px 14px;border-radius:8px;margin-bottom:14px}
.mv-status.success{background:rgba(63,185,80,.1);border:1px solid #3fb950;color:#3fb950}
.mv-status.error{background:rgba(248,81,73,.1);border:1px solid #f85149;color:#f85149}
.mv-status.info{background:rgba(56,189,248,.1);border:1px solid #38bdf8;color:#38bdf8}
.mv-upload-area{border:2px dashed #30363d;border-radius:10px;padding:20px;text-align:center;cursor:pointer;transition:all .2s;color:#8b949e;font-size:.82rem;font-family:'JetBrains Mono',monospace;margin-bottom:14px}
.mv-upload-area:hover,.mv-upload-area.dragover{border-color:#38bdf8;color:#38bdf8;background:rgba(56,189,248,.04)}
.mv-formats{display:flex;gap:6px;flex-wrap:wrap;margin-top:8px;justify-content:center}
.mv-format-tag{background:#21262d;color:#8b949e;font-family:'JetBrains Mono',monospace;font-size:.7rem;padding:3px 8px;border-radius:4px;border:1px solid #30363d}
.mv-info-bar{display:flex;gap:16px;font-family:'JetBrains Mono',monospace;font-size:.72rem;color:#8b949e;padding:10px 14px;background:#161b22;border-radius:8px;border:1px solid #21262d;flex-wrap:wrap;margin-bottom:12px}
.mv-info-bar span{color:#38bdf8;font-weight:600}
.mv-divider{text-align:center;color:#30363d;font-size:.75rem;font-family:'JetBrains Mono',monospace;margin:10px 0;position:relative}
.mv-divider::before,.mv-divider::after{content:'';position:absolute;top:50%;width:44%;height:1px;background:#21262d}
.mv-divider::before{left:0}
.mv-divider::after{right:0}
</style>

<div class="mv-wrap">
  <div class="mv-title">&#9883;&#65039; Molecule Viewer</div>
  <div class="mv-subtitle">// interactive 3D structure visualization &middot; powered by 3Dmol.js</div>

  <div class="mv-upload-area" id="mv-drop-zone">
    <div style="font-size:1.8rem;margin-bottom:8px">&#128194;</div>
    <div>Drop file here or <strong style="color:#38bdf8;cursor:pointer" id="mv-upload-btn">click to upload</strong></div>
    <div class="mv-formats">
      <span class="mv-format-tag">.xyz</span>
      <span class="mv-format-tag">.log</span>
      <span class="mv-format-tag">.pdb</span>
      <span class="mv-format-tag">.mol</span>
      <span class="mv-format-tag">.sdf</span>
      <span class="mv-format-tag">.mol2</span>
    </div>
  </div>
  <input type="file" id="mv-file-input" style="display:none" accept=".xyz,.log,.pdb,.mol,.sdf,.mol2,.gjf,.com,.cif">

  <div class="mv-divider">&#8212; or paste coordinates below &#8212;</div>

  <textarea class="mv-textarea" id="mv-text-input" placeholder="Paste XYZ, PDB, MOL, SDF, Gaussian LOG, or SMILES here...

Example XYZ:
3
water
O  0.000  0.000  0.000
H  0.757  0.586  0.000
H -0.757  0.586  0.000"></textarea>

  <div class="mv-controls-row" style="margin-top:12px">
    <div class="mv-field">
      <div class="mv-label">FORMAT</div>
      <select class="mv-select" id="mv-format">
        <option value="auto">Auto Detect</option>
        <option value="xyz">XYZ</option>
        <option value="pdb">PDB</option>
        <option value="mol">MOL / SDF</option>
        <option value="mol2">MOL2</option>
        <option value="sdf">SDF</option>
        <option value="smiles">SMILES</option>
      </select>
    </div>
    <div class="mv-field">
      <div class="mv-label">STYLE</div>
      <select class="mv-select" id="mv-style">
        <option value="stick">Stick</option>
        <option value="ballstick" selected>Ball and Stick</option>
        <option value="sphere">Space Filling (CPK)</option>
        <option value="line">Line</option>
        <option value="cross">Cross</option>
      </select>
    </div>
    <div class="mv-field">
      <div class="mv-label">COLOUR</div>
      <select class="mv-select" id="mv-color">
        <option value="Jmol" selected>By Element (CPK)</option>
        <option value="chain">By Chain</option>
        <option value="residue">By Residue</option>
        <option value="spectrum">Rainbow</option>
      </select>
    </div>
    <div class="mv-field">
      <div class="mv-label">SURFACE</div>
      <select class="mv-select" id="mv-surface">
        <option value="none">No Surface</option>
        <option value="VDW">Van der Waals</option>
        <option value="SAS">Solvent Accessible</option>
        <option value="SES">Solvent Excluded</option>
      </select>
    </div>
  </div>

  <div class="mv-controls-row">
    <div class="mv-toggle" id="mv-labels-toggle" onclick="mvToggleLabels()">&#127991;&#65039; Atom Labels</div>
    <div class="mv-toggle" id="mv-spin-toggle" onclick="mvToggleSpin()">&#128260; Auto Spin</div>
    <div class="mv-toggle" id="mv-bg-toggle" onclick="mvToggleBg()">&#127761; Light BG</div>
    <button class="mv-btn mv-btn-primary" onclick="mvLoad()">&#9654; Visualise</button>
    <button class="mv-btn mv-btn-secondary" onclick="mvSave()">&#128190; Save PNG</button>
    <button class="mv-btn mv-btn-danger" onclick="mvClear()">&#10005; Clear</button>
  </div>

  <div class="mv-status" id="mv-status" style="display:none"></div>
  <div class="mv-info-bar" id="mv-info-bar" style="display:none">
    Atoms: <span id="mv-atoms">&#8212;</span> &nbsp;|&nbsp;
    Format: <span id="mv-fmt">&#8212;</span> &nbsp;|&nbsp;
    Name: <span id="mv-name">&#8212;</span>
  </div>

  <div class="mv-viewer-box" id="mv-container">
    <div class="mv-placeholder">
      <div class="mv-placeholder-icon">&#9883;&#65039;</div>
      <div>Load a molecule to begin</div>
      <div style="font-size:.7rem;margin-top:4px;color:#21262d">XYZ &middot; PDB &middot; MOL &middot; SDF &middot; LOG &middot; SMILES</div>
    </div>
  </div>

  <div style="font-size:.72rem;color:#30363d;font-family:'JetBrains Mono',monospace;text-align:center;margin-top:4px">
    Drag to rotate &middot; Scroll to zoom &middot; Right-click drag to translate
  </div>
</div>

<script src="https://3dmol.org/build/3Dmol-min.js"></script>
<script>
(function(){
  var gViewer = null;
  var gLabels = false;
  var gSpin   = false;
  var gBg     = false;
  var gData   = null;
  var gFmt    = null;

  function mvStatus(msg, type){
    var el = document.getElementById('mv-status');
    el.textContent = msg;
    el.className = 'mv-status ' + type;
    el.style.display = 'block';
    setTimeout(function(){ el.style.display = 'none'; }, 6000);
  }

  function detectFmt(text, name){
    if(name){
      var ext = name.split('.').pop().toLowerCase();
      if(ext === 'pdb')  return 'pdb';
      if(ext === 'mol2') return 'mol2';
      if(ext === 'sdf')  return 'sdf';
      if(ext === 'mol')  return 'mol';
      if(ext === 'xyz')  return 'xyz';
      if(ext === 'log' || ext === 'gjf' || ext === 'com') return 'xyz';
    }
    if(/^(ATOM  |HETATM|REMARK|HEADER|CRYST)/m.test(text)) return 'pdb';
    if(/@<TRIPOS>MOLECULE/.test(text)) return 'mol2';
    if(/\$\$\$\$/.test(text)) return 'sdf';
    if(/(V2000|V3000|M  END)/.test(text)) return 'mol';
    if(/^\s*\d+\s*[\r\n]/.test(text)) return 'xyz';
    return 'xyz';
  }

  function buildViewer(){
    var cont = document.getElementById('mv-container');
    cont.innerHTML = '';
    var bg = gBg ? '#f8fafc' : '#010409';
    gViewer = $3Dmol.createViewer(cont, {
      backgroundColor: bg,
      antialias: true
    });
    return gViewer;
  }

  function applyStyle(){
    if(!gViewer) return;
    try { gViewer.removeAllSurfaces(); } catch(e){}
    try { gViewer.removeAllLabels(); } catch(e){}
    var style  = document.getElementById('mv-style').value;
    var color  = document.getElementById('mv-color').value;
    var surf   = document.getElementById('mv-surface').value;
    var cOpt   = {colorscheme: color};

    gViewer.setStyle({});
    if(style === 'ballstick'){
      gViewer.setStyle({}, {stick: Object.assign({radius:0.15}, cOpt), sphere: Object.assign({radius:0.35}, cOpt)});
    } else if(style === 'stick'){
      gViewer.setStyle({}, {stick: Object.assign({radius:0.2}, cOpt)});
    } else if(style === 'sphere'){
      gViewer.setStyle({}, {sphere: Object.assign({scale:1.0}, cOpt)});
    } else if(style === 'line'){
      gViewer.setStyle({}, {line: cOpt});
    } else if(style === 'cross'){
      gViewer.setStyle({}, {cross: Object.assign({radius:0.3}, cOpt)});
    }

    if(surf !== 'none'){
      try {
        gViewer.addSurface($3Dmol.SurfaceType[surf], {opacity:0.4, colorscheme:color});
      } catch(e){}
    }

    if(gLabels){
      gViewer.addPropertyLabels('elem', {}, {
        font: 'Arial', fontSize: 12, fontColor: 'white',
        showBackground: false, alignment: 'center'
      });
    }

    gViewer.spin(gSpin);
    gViewer.render();
  }

  function doLoad(text, fmt, name){
    if(typeof $3Dmol === 'undefined'){
      mvStatus('&#10007; 3Dmol.js not loaded yet — please wait a moment and try again', 'error');
      return;
    }
    try {
      buildViewer();
      gViewer.addModel(text, fmt);
      applyStyle();
      gViewer.zoomTo();
      gViewer.render();
      var atoms = gViewer.getModel().selectedAtoms({});
      document.getElementById('mv-atoms').textContent = atoms.length;
      document.getElementById('mv-fmt').textContent   = fmt.toUpperCase();
      document.getElementById('mv-name').textContent  = name || 'unknown';
      document.getElementById('mv-info-bar').style.display = 'flex';
      gData = text; gFmt = fmt;
      mvStatus('&#10003; Loaded successfully — ' + atoms.length + ' atoms', 'success');
    } catch(e) {
      mvStatus('&#10007; Error rendering: ' + e.message, 'error');
      console.error('3Dmol error:', e);
    }
  }

  window.mvLoad = function(){
    var text = document.getElementById('mv-text-input').value.trim();
    if(!text){ mvStatus('Please paste coordinates or upload a file first', 'info'); return; }
    var sel = document.getElementById('mv-format').value;
    var fmt = (sel === 'auto') ? detectFmt(text, null) : sel;
    if(fmt === 'smiles'){
      mvStatus('Fetching 3D structure from NIH CACTUS...', 'info');
      var url = 'https://cactus.nci.nih.gov/chemical/structure/' + encodeURIComponent(text) + '/file?format=sdf&get3d=true';
      fetch(url)
        .then(function(r){ return r.text(); })
        .then(function(sdf){
          if(sdf.toLowerCase().indexOf('not found') > -1 || sdf.trim().length < 20){
            mvStatus('Could not resolve SMILES. Try pasting as XYZ or SDF instead.', 'error');
          } else {
            document.getElementById('mv-text-input').value = sdf;
            doLoad(sdf, 'sdf', text);
          }
        })
        .catch(function(){ mvStatus('Network error resolving SMILES', 'error'); });
      return;
    }
    doLoad(text, fmt, 'Pasted coordinates');
  };

  window.mvSave = function(){
    if(!gViewer){ mvStatus('No molecule loaded', 'info'); return; }
    gViewer.pngURI(function(uri){
      var a = document.createElement('a');
      a.href = uri; a.download = 'molecule_GMP.png'; a.click();
    }, 2048);
  };

  window.mvClear = function(){
    if(gViewer){ try{ gViewer.clear(); gViewer.render(); }catch(e){} }
    gViewer = null; gData = null; gFmt = null;
    gLabels = false; gSpin = false;
    document.getElementById('mv-text-input').value = '';
    document.getElementById('mv-status').style.display = 'none';
    document.getElementById('mv-info-bar').style.display = 'none';
    document.getElementById('mv-spin-toggle').classList.remove('active');
    document.getElementById('mv-labels-toggle').classList.remove('active');
    var cont = document.getElementById('mv-container');
    cont.innerHTML = '<div class="mv-placeholder"><div class="mv-placeholder-icon">&#9883;&#65039;</div><div>Load a molecule to begin</div></div>';
  };

  window.mvToggleLabels = function(){
    gLabels = !gLabels;
    document.getElementById('mv-labels-toggle').classList.toggle('active', gLabels);
    if(gViewer && gData) applyStyle();
  };

  window.mvToggleSpin = function(){
    gSpin = !gSpin;
    document.getElementById('mv-spin-toggle').classList.toggle('active', gSpin);
    if(gViewer) gViewer.spin(gSpin);
  };

  window.mvToggleBg = function(){
    gBg = !gBg;
    document.getElementById('mv-bg-toggle').classList.toggle('active', gBg);
    if(gViewer && gData){
      doLoad(gData, gFmt, document.getElementById('mv-name').textContent);
    }
  };

  // Style dropdowns — re-render on change
  ['mv-style','mv-color','mv-surface'].forEach(function(id){
    document.getElementById(id).addEventListener('change', function(){
      if(gViewer && gData) applyStyle();
    });
  });

  // File input handler
  document.getElementById('mv-file-input').addEventListener('change', function(e){
    var file = e.target.files[0];
    if(!file) return;
    var reader = new FileReader();
    reader.onload = function(ev){
      var text = ev.target.result;
      document.getElementById('mv-text-input').value = text;
      var sel = document.getElementById('mv-format').value;
      var fmt = (sel === 'auto') ? detectFmt(text, file.name) : sel;
      doLoad(text, fmt, file.name);
    };
    reader.readAsText(file);
    e.target.value = '';
  });

  // Upload button click
  document.getElementById('mv-upload-btn').addEventListener('click', function(e){
    e.stopPropagation();
    document.getElementById('mv-file-input').click();
  });

  // Drop zone click
  document.getElementById('mv-drop-zone').addEventListener('click', function(){
    document.getElementById('mv-file-input').click();
  });

  // Drag and drop
  var dz = document.getElementById('mv-drop-zone');
  dz.addEventListener('dragover', function(e){ e.preventDefault(); dz.classList.add('dragover'); });
  dz.addEventListener('dragleave', function(){ dz.classList.remove('dragover'); });
  dz.addEventListener('drop', function(e){
    e.preventDefault(); dz.classList.remove('dragover');
    var file = e.dataTransfer.files[0];
    if(!file) return;
    var reader = new FileReader();
    reader.onload = function(ev){
      var text = ev.target.result;
      document.getElementById('mv-text-input').value = text;
      var sel = document.getElementById('mv-format').value;
      var fmt = (sel === 'auto') ? detectFmt(text, file.name) : sel;
      doLoad(text, fmt, file.name);
    };
    reader.readAsText(file);
  });

})();
</script>
