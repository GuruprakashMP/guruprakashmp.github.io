---
layout: single
title: "Molecule Viewer"
permalink: /molviewer/
author_profile: true
---

<style>
  @import url('https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;600&family=Syne:wght@400;600;700&display=swap');

  .mv-wrap {
    font-family: 'Syne', sans-serif;
    background: #0d1117;
    border-radius: 16px;
    padding: 28px;
    color: #e6edf3;
    margin: 0 -12px;
    box-shadow: 0 0 60px rgba(56,189,248,0.07);
  }

  .mv-title {
    font-size: 1.3rem;
    font-weight: 700;
    color: #38bdf8;
    letter-spacing: 0.04em;
    margin-bottom: 6px;
  }

  .mv-subtitle {
    font-size: 0.82rem;
    color: #8b949e;
    margin-bottom: 22px;
    font-family: 'JetBrains Mono', monospace;
  }

  .mv-input-row {
    display: flex;
    gap: 12px;
    margin-bottom: 14px;
    flex-wrap: wrap;
  }

  .mv-textarea {
    width: 100%;
    min-height: 130px;
    background: #161b22;
    border: 1.5px solid #30363d;
    border-radius: 10px;
    color: #c9d1d9;
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.78rem;
    padding: 12px;
    resize: vertical;
    outline: none;
    transition: border 0.2s;
    box-sizing: border-box;
  }

  .mv-textarea:focus {
    border-color: #38bdf8;
  }

  .mv-select {
    background: #161b22;
    border: 1.5px solid #30363d;
    border-radius: 8px;
    color: #c9d1d9;
    font-family: 'Syne', sans-serif;
    font-size: 0.82rem;
    padding: 8px 12px;
    outline: none;
    cursor: pointer;
    transition: border 0.2s;
    flex: 1;
    min-width: 130px;
  }

  .mv-select:focus { border-color: #38bdf8; }

  .mv-label {
    font-size: 0.75rem;
    color: #8b949e;
    margin-bottom: 5px;
    font-family: 'JetBrains Mono', monospace;
    letter-spacing: 0.05em;
  }

  .mv-field { flex: 1; min-width: 130px; }

  .mv-btn {
    padding: 10px 22px;
    border-radius: 8px;
    border: none;
    font-family: 'Syne', sans-serif;
    font-weight: 600;
    font-size: 0.85rem;
    cursor: pointer;
    transition: all 0.2s;
    letter-spacing: 0.03em;
  }

  .mv-btn-primary {
    background: linear-gradient(135deg, #38bdf8, #0ea5e9);
    color: #0d1117;
  }

  .mv-btn-primary:hover { transform: translateY(-1px); box-shadow: 0 4px 20px rgba(56,189,248,0.35); }

  .mv-btn-secondary {
    background: #21262d;
    color: #c9d1d9;
    border: 1.5px solid #30363d;
  }

  .mv-btn-secondary:hover { border-color: #38bdf8; color: #38bdf8; }

  .mv-btn-danger {
    background: #21262d;
    color: #f85149;
    border: 1.5px solid #30363d;
  }

  .mv-btn-danger:hover { border-color: #f85149; }

  .mv-viewer-box {
    width: 100%;
    height: 460px;
    background: #010409;
    border-radius: 12px;
    border: 1.5px solid #21262d;
    position: relative;
    overflow: hidden;
    margin: 18px 0;
  }

  .mv-viewer-box canvas { border-radius: 12px; }

  .mv-placeholder {
    position: absolute;
    inset: 0;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    color: #30363d;
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.85rem;
    text-align: center;
    gap: 14px;
    pointer-events: none;
  }

  .mv-placeholder-icon { font-size: 3rem; opacity: 0.4; }

  .mv-controls-row {
    display: flex;
    gap: 10px;
    flex-wrap: wrap;
    align-items: flex-end;
    margin-bottom: 14px;
  }

  .mv-toggle {
    display: flex;
    align-items: center;
    gap: 7px;
    font-size: 0.8rem;
    color: #8b949e;
    cursor: pointer;
    padding: 8px 14px;
    background: #161b22;
    border: 1.5px solid #30363d;
    border-radius: 8px;
    transition: all 0.2s;
    user-select: none;
  }

  .mv-toggle:hover { border-color: #38bdf8; color: #38bdf8; }
  .mv-toggle.active { border-color: #38bdf8; color: #38bdf8; background: rgba(56,189,248,0.08); }

  .mv-toggle input { display: none; }

  .mv-status {
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.75rem;
    padding: 10px 14px;
    border-radius: 8px;
    margin-bottom: 14px;
    display: none;
  }

  .mv-status.success { background: rgba(63,185,80,0.1); border: 1px solid #3fb950; color: #3fb950; display: block; }
  .mv-status.error { background: rgba(248,81,73,0.1); border: 1px solid #f85149; color: #f85149; display: block; }
  .mv-status.info { background: rgba(56,189,248,0.1); border: 1px solid #38bdf8; color: #38bdf8; display: block; }

  .mv-upload-area {
    border: 2px dashed #30363d;
    border-radius: 10px;
    padding: 20px;
    text-align: center;
    cursor: pointer;
    transition: all 0.2s;
    color: #8b949e;
    font-size: 0.82rem;
    font-family: 'JetBrains Mono', monospace;
    margin-bottom: 14px;
  }

  .mv-upload-area:hover { border-color: #38bdf8; color: #38bdf8; background: rgba(56,189,248,0.04); }
  .mv-upload-area.dragover { border-color: #38bdf8; background: rgba(56,189,248,0.08); }

  .mv-divider {
    text-align: center;
    color: #30363d;
    font-size: 0.75rem;
    font-family: 'JetBrains Mono', monospace;
    margin: 10px 0;
    position: relative;
  }

  .mv-divider::before, .mv-divider::after {
    content: '';
    position: absolute;
    top: 50%;
    width: 44%;
    height: 1px;
    background: #21262d;
  }

  .mv-divider::before { left: 0; }
  .mv-divider::after { right: 0; }

  .mv-formats {
    display: flex;
    gap: 6px;
    flex-wrap: wrap;
    margin-top: 8px;
  }

  .mv-format-tag {
    background: #21262d;
    color: #8b949e;
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.7rem;
    padding: 3px 8px;
    border-radius: 4px;
    border: 1px solid #30363d;
  }

  #mv-file-input { display: none; }

  .mv-info-bar {
    display: flex;
    gap: 16px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 0.72rem;
    color: #8b949e;
    padding: 10px 14px;
    background: #161b22;
    border-radius: 8px;
    border: 1px solid #21262d;
    flex-wrap: wrap;
  }

  .mv-info-item span { color: #38bdf8; font-weight: 600; }
</style>

<div class="mv-wrap">
  <div class="mv-title">⚗️ Molecule Viewer</div>
  <div class="mv-subtitle">// interactive 3D structure visualization for chemistry</div>

  <!-- Upload area -->
  <div class="mv-upload-area" id="mv-drop-zone" onclick="document.getElementById('mv-file-input').click()">
    <div style="font-size:1.8rem; margin-bottom:8px;">📂</div>
    <div>Drop file here or click to upload</div>
    <div class="mv-formats">
      <span class="mv-format-tag">.xyz</span>
      <span class="mv-format-tag">.log</span>
      <span class="mv-format-tag">.pdb</span>
      <span class="mv-format-tag">.mol</span>
      <span class="mv-format-tag">.sdf</span>
      <span class="mv-format-tag">.mol2</span>
      <span class="mv-format-tag">.cif</span>
    </div>
  </div>
  <input type="file" id="mv-file-input" accept=".xyz,.log,.pdb,.mol,.sdf,.mol2,.cif,.gjf,.com">

  <div class="mv-divider">or paste coordinates / SMILES below</div>

  <!-- Text input -->
  <textarea class="mv-textarea" id="mv-text-input" placeholder="Paste XYZ, PDB, MOL, SDF, Gaussian LOG, or SMILES text here...&#10;&#10;Example XYZ:&#10;3&#10;water&#10;O  0.000  0.000  0.000&#10;H  0.757  0.586  0.000&#10;H -0.757  0.586  0.000"></textarea>

  <!-- Format + Style controls -->
  <div class="mv-controls-row" style="margin-top:12px;">
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
        <option value="ballstick" selected>Ball & Stick</option>
        <option value="sphere">Space Filling (CPK)</option>
        <option value="line">Line</option>
        <option value="cross">Cross</option>
      </select>
    </div>
    <div class="mv-field">
      <div class="mv-label">COLOUR SCHEME</div>
      <select class="mv-select" id="mv-color">
        <option value="element" selected>By Element (CPK)</option>
        <option value="chain">By Chain</option>
        <option value="residue">By Residue</option>
        <option value="b">By B-factor</option>
        <option value="spectrum">Rainbow Spectrum</option>
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

  <!-- Toggles + Buttons row -->
  <div class="mv-controls-row">
    <label class="mv-toggle" id="mv-labels-toggle" onclick="toggleLabels()">
      <span>🏷️</span> Atom Labels
    </label>
    <label class="mv-toggle" id="mv-spin-toggle" onclick="toggleSpin()">
      <span>🔄</span> Auto Spin
    </label>
    <label class="mv-toggle" id="mv-bg-toggle" onclick="toggleBg()">
      <span>🌗</span> Light BG
    </label>
    <button class="mv-btn mv-btn-primary" onclick="loadMolecule()">▶ Visualise</button>
    <button class="mv-btn mv-btn-secondary" onclick="downloadImage()">💾 Save PNG</button>
    <button class="mv-btn mv-btn-danger" onclick="clearViewer()">✕ Clear</button>
  </div>

  <!-- Status -->
  <div class="mv-status" id="mv-status"></div>

  <!-- Info bar -->
  <div class="mv-info-bar" id="mv-info-bar" style="display:none; margin-bottom:12px;">
    <div>Atoms: <span id="mv-atom-count">—</span></div>
    <div>Format: <span id="mv-detected-format">—</span></div>
    <div>Name: <span id="mv-mol-name">—</span></div>
  </div>

  <!-- 3D Viewer -->
  <div class="mv-viewer-box" id="mv-container">
    <div class="mv-placeholder" id="mv-placeholder">
      <div class="mv-placeholder-icon">⚛️</div>
      <div>Load a molecule to begin</div>
      <div style="font-size:0.7rem; margin-top:4px; color:#21262d;">Supports XYZ · PDB · MOL · SDF · LOG · SMILES</div>
    </div>
  </div>

  <div style="font-size:0.72rem; color:#30363d; font-family:'JetBrains Mono',monospace; text-align:center;">
    Drag to rotate · Scroll to zoom · Right-click drag to translate · Powered by 3Dmol.js
  </div>
</div>

<!-- Load 3Dmol.js -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<script src="https://3dmol.org/build/3Dmol-min.js"></script>

<script>
  var viewer = null;
  var showLabels = false;
  var spinning = false;
  var lightBg = false;
  var currentMolData = null;
  var currentFormat = null;

  function initViewer(bg) {
    var bgColor = bg || '#010409';
    var container = document.getElementById('mv-container');
    container.innerHTML = '';
    var placeholder = document.createElement('div');
    placeholder.className = 'mv-placeholder';
    placeholder.id = 'mv-placeholder';
    placeholder.style.display = 'none';
    container.appendChild(placeholder);

    viewer = $3Dmol.createViewer($(container), {
      backgroundColor: bgColor,
      antialias: true,
      id: 'mv-viewer'
    });
    return viewer;
  }

  function showStatus(msg, type) {
    var s = document.getElementById('mv-status');
    s.textContent = msg;
    s.className = 'mv-status ' + type;
    setTimeout(function(){ if(s.textContent === msg) s.style.display = 'none'; }, 4000);
  }

  function detectFormat(text, filename) {
    if (filename) {
      var ext = filename.split('.').pop().toLowerCase();
      if (ext === 'xyz') return 'xyz';
      if (ext === 'pdb') return 'pdb';
      if (ext === 'mol' || ext === 'mdl') return 'mol';
      if (ext === 'sdf') return 'sdf';
      if (ext === 'mol2') return 'mol2';
      if (ext === 'log' || ext === 'gjf' || ext === 'com') return 'xyz'; // parse as xyz from Gaussian
    }
    // Auto detect from content
    if (text.match(/^ATOM|^HETATM|^REMARK|^HEADER/m)) return 'pdb';
    if (text.match(/@<TRIPOS>MOLECULE/)) return 'mol2';
    if (text.match(/\$\$\$\$/)) return 'sdf';
    if (text.match(/^\s*\d+\s*\n.*\n\s*[A-Z][a-z]?\s+[-\d.]+/s)) return 'xyz';
    if (text.match(/V2000|V3000|M\s+END/)) return 'mol';
    // Try SMILES (no newlines, has organic atoms)
    if (!text.includes('\n') && text.match(/^[CNOPSFIBrClcnosp\[\]@+\-=#\(\)\/\\%\d]+$/)) return 'smiles';
    return 'xyz';
  }

  function applyStyle() {
    if (!viewer) return;
    viewer.removeAllSurfaces();
    var style = document.getElementById('mv-style').value;
    var colorScheme = document.getElementById('mv-color').value;
    var surface = document.getElementById('mv-surface').value;

    var colorOpt = {};
    if (colorScheme === 'element') colorOpt = {colorscheme: 'Jmol'};
    else if (colorScheme === 'chain') colorOpt = {colorscheme: 'chain'};
    else if (colorScheme === 'residue') colorOpt = {colorscheme: 'residue'};
    else if (colorScheme === 'b') colorOpt = {colorscheme: {gradient:'rwb', prop:'b'}};
    else if (colorScheme === 'spectrum') colorOpt = {colorscheme: {gradient:'spectrum'}};

    viewer.setStyle({});
    if (style === 'ballstick') {
      viewer.setStyle({}, {stick: Object.assign({radius:0.15}, colorOpt), sphere: Object.assign({radius:0.35}, colorOpt)});
    } else if (style === 'stick') {
      viewer.setStyle({}, {stick: Object.assign({radius:0.2}, colorOpt)});
    } else if (style === 'sphere') {
      viewer.setStyle({}, {sphere: Object.assign({scale:1.0}, colorOpt)});
    } else if (style === 'line') {
      viewer.setStyle({}, {line: colorOpt});
    } else if (style === 'cross') {
      viewer.setStyle({}, {cross: Object.assign({radius:0.3}, colorOpt)});
    }

    if (surface !== 'none') {
      viewer.addSurface($3Dmol.SurfaceType[surface], {
        opacity: 0.45,
        colorscheme: colorScheme === 'element' ? 'Jmol' : colorScheme
      });
    }

    if (showLabels) addLabels();
    viewer.render();
  }

  function addLabels() {
    viewer.removeAllLabels();
    viewer.mapAtomProperties({});
    viewer.addPropertyLabels('elem', {}, {
      font: 'JetBrains Mono',
      fontSize: 11,
      fontColor: 'white',
      showBackground: false,
      alignment: 'center'
    });
  }

  function loadMoleculeFromData(data, format, name) {
    try {
      var bg = lightBg ? '#f8f9fa' : '#010409';
      initViewer(bg);
      document.getElementById('mv-placeholder').style.display = 'none';

      var fmt = format || 'xyz';
      viewer.addModel(data, fmt);
      applyStyle();
      viewer.zoomTo();
      viewer.render();
      if (spinning) viewer.spin(true);

      // Info bar
      var atoms = viewer.getModel().selectedAtoms({});
      document.getElementById('mv-atom-count').textContent = atoms.length;
      document.getElementById('mv-detected-format').textContent = fmt.toUpperCase();
      document.getElementById('mv-mol-name').textContent = name || '—';
      document.getElementById('mv-info-bar').style.display = 'flex';

      currentMolData = data;
      currentFormat = fmt;
      showStatus('✓ Molecule loaded — ' + atoms.length + ' atoms', 'success');
    } catch(e) {
      showStatus('✗ Error loading molecule: ' + e.message, 'error');
    }
  }

  function loadMolecule() {
    var text = document.getElementById('mv-text-input').value.trim();
    if (!text) { showStatus('⚠ Please paste coordinates or upload a file', 'info'); return; }
    var fmtSelect = document.getElementById('mv-format').value;
    var fmt = fmtSelect === 'auto' ? detectFormat(text, null) : fmtSelect;

    // Handle SMILES via NIH resolver
    if (fmt === 'smiles') {
      showStatus('⏳ Fetching 3D structure from NIH resolver...', 'info');
      var url = 'https://cactus.nci.nih.gov/chemical/structure/' + encodeURIComponent(text) + '/file?format=sdf&get3d=true';
      fetch(url).then(function(r){ return r.text(); }).then(function(sdf){
        if (sdf.includes('Page not found') || sdf.includes('error')) {
          showStatus('✗ Could not resolve SMILES. Try a different identifier.', 'error');
        } else {
          loadMoleculeFromData(sdf, 'sdf', text);
        }
      }).catch(function(){ showStatus('✗ Network error resolving SMILES', 'error'); });
      return;
    }
    loadMoleculeFromData(text, fmt, 'Pasted');
  }

  function downloadImage() {
    if (!viewer) { showStatus('⚠ No molecule loaded', 'info'); return; }
    viewer.pngURI(function(uri){
      var a = document.createElement('a');
      a.href = uri;
      a.download = 'molecule_GMP.png';
      a.click();
    }, 2048);
  }

  function clearViewer() {
    if (viewer) { viewer.clear(); viewer.render(); }
    document.getElementById('mv-text-input').value = '';
    document.getElementById('mv-status').style.display = 'none';
    document.getElementById('mv-info-bar').style.display = 'none';
    document.getElementById('mv-container').innerHTML = '<div class="mv-placeholder" id="mv-placeholder"><div class="mv-placeholder-icon">⚛️</div><div>Load a molecule to begin</div></div>';
    viewer = null;
    spinning = false;
    showLabels = false;
    document.getElementById('mv-spin-toggle').classList.remove('active');
    document.getElementById('mv-labels-toggle').classList.remove('active');
    currentMolData = null;
  }

  function toggleLabels() {
    showLabels = !showLabels;
    document.getElementById('mv-labels-toggle').classList.toggle('active', showLabels);
    if (viewer && currentMolData) applyStyle();
  }

  function toggleSpin() {
    spinning = !spinning;
    document.getElementById('mv-spin-toggle').classList.toggle('active', spinning);
    if (viewer) viewer.spin(spinning);
  }

  function toggleBg() {
    lightBg = !lightBg;
    document.getElementById('mv-bg-toggle').classList.toggle('active', lightBg);
    if (viewer && currentMolData) {
      loadMoleculeFromData(currentMolData, currentFormat, document.getElementById('mv-mol-name').textContent);
    }
    document.getElementById('mv-container').style.background = lightBg ? '#f8f9fa' : '#010409';
  }

  // Re-render on style/color/surface change
  ['mv-style','mv-color','mv-surface'].forEach(function(id){
    document.getElementById(id).addEventListener('change', function(){
      if (viewer && currentMolData) applyStyle();
    });
  });

  // File upload
  document.getElementById('mv-file-input').addEventListener('change', function(e){
    var file = e.target.files[0];
    if (!file) return;
    var reader = new FileReader();
    reader.onload = function(ev){
      var text = ev.target.result;
      document.getElementById('mv-text-input').value = text;
      var fmtSelect = document.getElementById('mv-format').value;
      var fmt = fmtSelect === 'auto' ? detectFormat(text, file.name) : fmtSelect;
      loadMoleculeFromData(text, fmt, file.name);
    };
    reader.readAsText(file);
  });

  // Drag and drop
  var dz = document.getElementById('mv-drop-zone');
  dz.addEventListener('dragover', function(e){ e.preventDefault(); dz.classList.add('dragover'); });
  dz.addEventListener('dragleave', function(){ dz.classList.remove('dragover'); });
  dz.addEventListener('drop', function(e){
    e.preventDefault();
    dz.classList.remove('dragover');
    var file = e.dataTransfer.files[0];
    if (!file) return;
    var reader = new FileReader();
    reader.onload = function(ev){
      var text = ev.target.result;
      document.getElementById('mv-text-input').value = text;
      var fmtSelect = document.getElementById('mv-format').value;
      var fmt = fmtSelect === 'auto' ? detectFormat(text, file.name) : fmtSelect;
      loadMoleculeFromData(text, fmt, file.name);
    };
    reader.readAsText(file);
  });
</script>
