<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>CV Builder</title>
<style>
  :root{
    --ink:#1B2430;
    --paper:#FAF8F4;
    --accent:#3F6659;
    --rule:#D8D3C8;
    --muted:#5B6570;
    --panel:#FFFFFF;
    --danger:#A8443A;
  }

  *{box-sizing:border-box;}
  html,body{margin:0;padding:0;}
  body{
    background:var(--paper);
    color:var(--ink);
    font-family:'IBM Plex Sans', -apple-system, Segoe UI, Roboto, sans-serif;
    font-size:15px;
    line-height:1.5;
  }

  h1,h2,h3, .cv-name{
    font-family:'Source Serif 4', Georgia, 'Times New Roman', serif;
  }

  /* ===== App shell ===== */
  .app{
    display:grid;
    grid-template-columns: 420px 1fr;
    min-height:100vh;
  }
  @media (max-width: 900px){
    .app{grid-template-columns:1fr;}
  }

  /* ===== Form panel ===== */
  .form-panel{
    background:var(--panel);
    border-right:1px solid var(--rule);
    padding:28px 28px 80px;
    overflow-y:auto;
    max-height:100vh;
  }
  @media (max-width:900px){
    .form-panel{max-height:none; border-right:none; border-bottom:1px solid var(--rule);}
  }

  .brand{
    display:flex;
    align-items:baseline;
    gap:10px;
    margin-bottom:6px;
  }
  .brand h1{
    font-size:22px;
    margin:0;
    letter-spacing:0.2px;
  }
  .brand span{
    color:var(--muted);
    font-size:13px;
  }
  .form-intro{
    color:var(--muted);
    font-size:13px;
    margin:0 0 28px;
    max-width:34ch;
  }

  fieldset{
    border:none;
    padding:0;
    margin:0 0 30px;
  }
  legend{
    font-size:13px;
    font-weight:600;
    text-transform:none;
    color:var(--ink);
    padding:0 0 10px;
    border-bottom:1px solid var(--rule);
    width:100%;
    margin-bottom:16px;
  }

  label{
    display:block;
    font-size:12.5px;
    color:var(--muted);
    margin-bottom:5px;
  }
  .field{margin-bottom:14px;}
  .row2{
    display:grid;
    grid-template-columns:1fr 1fr;
    gap:12px;
  }

  input[type=text], input[type=email], input[type=tel], input[type=url], input[type=month], textarea{
    width:100%;
    padding:9px 11px;
    border:1px solid var(--rule);
    border-radius:3px;
    background:var(--paper);
    color:var(--ink);
    font-family:inherit;
    font-size:14px;
  }
  input:focus, textarea:focus{
    outline:2px solid var(--accent);
    outline-offset:1px;
    border-color:var(--accent);
  }
  textarea{resize:vertical; min-height:64px;}

  .entry-block{
    border:1px solid var(--rule);
    border-radius:4px;
    padding:14px 14px 4px;
    margin-bottom:12px;
    position:relative;
    background:#FCFBF8;
  }
  .entry-block .remove-btn{
    position:absolute;
    top:10px;
    right:10px;
    background:none;
    border:none;
    color:var(--muted);
    font-size:12.5px;
    cursor:pointer;
    text-decoration:underline;
    padding:2px 4px;
  }
  .entry-block .remove-btn:hover{color:var(--danger);}

  .add-btn{
    width:100%;
    padding:9px;
    background:none;
    border:1px dashed var(--rule);
    border-radius:4px;
    color:var(--accent);
    font-size:13.5px;
    font-weight:600;
    cursor:pointer;
    font-family:inherit;
  }
  .add-btn:hover{border-color:var(--accent); background:#F1F5F3;}

  .skills-hint{
    font-size:12px;
    color:var(--muted);
    margin-top:4px;
  }

  .theme-options{
    display:grid;
    grid-template-columns:1fr 1fr;
    gap:8px;
  }
  .theme-swatch{
    display:flex;
    align-items:center;
    gap:8px;
    padding:9px 10px;
    border:1px solid var(--rule);
    border-radius:4px;
    background:var(--paper);
    color:var(--ink);
    font-family:inherit;
    font-size:13px;
    cursor:pointer;
    text-align:left;
  }
  .theme-swatch:hover{border-color:var(--accent);}
  .theme-swatch.is-active{
    border-color:var(--accent);
    background:#F1F5F3;
    font-weight:600;
  }
  .swatch-color{
    width:14px;
    height:14px;
    border-radius:50%;
    flex:none;
    display:inline-block;
  }

  .action-row{
    display:grid;
    grid-template-columns:1fr 1fr;
    gap:10px;
  }
  .print-btn, .preview-btn{
    display:block;
    width:100%;
    padding:13px;
    border-radius:4px;
    font-size:14.5px;
    font-weight:600;
    cursor:pointer;
    font-family:inherit;
    letter-spacing:0.2px;
    border:none;
  }
  .print-btn{
    background:var(--ink);
    color:var(--paper);
  }
  .print-btn:hover{background:var(--accent);}
  .preview-btn{
    background:none;
    color:var(--ink);
    border:1px solid var(--rule);
  }
  .preview-btn:hover{border-color:var(--accent); color:var(--accent);}

  /* ===== Fullscreen preview modal ===== */
  .preview-modal{
    position:fixed;
    inset:0;
    background:rgba(27,36,48,0.55);
    display:none;
    align-items:flex-start;
    justify-content:center;
    padding:32px 16px;
    overflow-y:auto;
    z-index:100;
  }
  .preview-modal.is-open{display:flex;}
  .preview-modal-inner{
    background:#EDEAE2;
    border-radius:6px;
    padding:24px;
    width:100%;
    max-width:820px;
  }
  .preview-modal-bar{
    display:flex;
    justify-content:flex-end;
    gap:10px;
    margin-bottom:16px;
  }
  .preview-modal-bar button{
    padding:8px 16px;
    border-radius:4px;
    font-size:13.5px;
    font-weight:600;
    cursor:pointer;
    font-family:inherit;
    border:1px solid var(--rule);
    background:#fff;
    color:var(--ink);
  }
  .preview-modal-bar .close-btn:hover{border-color:var(--danger); color:var(--danger);}
  .preview-modal-bar .print-btn-modal{
    background:var(--ink);
    color:var(--paper);
    border:none;
  }
  .preview-modal-bar .print-btn-modal:hover{background:var(--accent);}
  @media print{
    .preview-modal.is-open{position:static; background:none; padding:0; inset:auto;}
    .preview-modal-bar{display:none !important;}
    .preview-modal.is-open .preview-modal-inner{background:#fff; padding:0; max-width:none; border-radius:0;}
  }

  /* ===== Preview panel ===== */
  .preview-panel{
    background:#EDEAE2;
    padding:40px;
    display:flex;
    justify-content:center;
    align-items:flex-start;
  }
  @media (max-width:900px){
    .preview-panel{padding:20px 14px;}
  }

  .page{
    /* CV theme tokens — default (Editorial) theme; overridden per .theme-* below */
    --cv-ink:#1B2430;
    --cv-accent:#3F6659;
    --cv-muted:#5B6570;
    --cv-rule:#D8D3C8;
    --cv-heading-font:'Source Serif 4', Georgia, 'Times New Roman', serif;
    --cv-label-transform:uppercase;
    --cv-label-spacing:1.2px;
    --cv-rule-weight:2px;

    background:#fff;
    width:100%;
    max-width:760px;
    min-height:1000px;
    padding:56px 60px;
    box-shadow:0 1px 3px rgba(0,0,0,0.08), 0 8px 24px rgba(0,0,0,0.08);
    color:var(--cv-ink);
  }
  @media (max-width:900px){
    .page{padding:32px 26px;}
  }

  /* ---- Theme variants ---- */
  .page.theme-classic{
    --cv-ink:#16233A;
    --cv-accent:#8A6D3B;
    --cv-muted:#5B6472;
    --cv-rule:#C9CEDA;
    --cv-heading-font:'Source Serif 4', Georgia, 'Times New Roman', serif;
    --cv-label-transform:uppercase;
    --cv-label-spacing:1.6px;
    --cv-rule-weight:2px;
  }
  .page.theme-modern{
    --cv-ink:#202124;
    --cv-accent:#2F5CD6;
    --cv-muted:#63666B;
    --cv-rule:#E1E2E5;
    --cv-heading-font:'IBM Plex Sans', -apple-system, Segoe UI, sans-serif;
    --cv-label-transform:none;
    --cv-label-spacing:0.2px;
    --cv-rule-weight:1px;
  }
  .page.theme-forest{
    --cv-ink:#1D2B22;
    --cv-accent:#4B7A4C;
    --cv-muted:#5C6B5F;
    --cv-rule:#D3DDCF;
    --cv-heading-font:'Source Serif 4', Georgia, serif;
    --cv-label-transform:uppercase;
    --cv-label-spacing:1.2px;
    --cv-rule-weight:2px;
  }
  .page.theme-mono{
    --cv-ink:#111111;
    --cv-accent:#111111;
    --cv-muted:#6B6B6B;
    --cv-rule:#CFCFCF;
    --cv-heading-font:'IBM Plex Sans', sans-serif;
    --cv-label-transform:uppercase;
    --cv-label-spacing:2px;
    --cv-rule-weight:1px;
  }

  .cv-name{
    font-family:var(--cv-heading-font);
    font-size:32px;
    margin:0 0 4px;
    color:var(--cv-ink);
  }
  .cv-title{
    font-size:15px;
    color:var(--cv-accent);
    margin:0 0 14px;
    font-weight:600;
  }
  .cv-contact{
    font-size:12.5px;
    color:var(--cv-muted);
    display:flex;
    flex-wrap:wrap;
    gap:2px 14px;
    margin-bottom:26px;
    padding-bottom:22px;
    border-bottom:var(--cv-rule-weight) solid var(--cv-ink);
  }
  .cv-contact span:empty{display:none;}

  .cv-section{margin-bottom:24px;}
  .cv-section:last-child{margin-bottom:0;}
  .cv-section h2{
    font-size:13px;
    text-transform:var(--cv-label-transform);
    letter-spacing:var(--cv-label-spacing);
    color:var(--cv-accent);
    margin:0 0 12px;
    font-weight:600;
    font-family:'IBM Plex Sans', sans-serif;
  }

  .cv-summary{
    font-size:14px;
    color:var(--cv-ink);
    margin:0;
  }

  .cv-item{margin-bottom:16px;}
  .cv-item:last-child{margin-bottom:0;}
  .cv-item-head{
    display:flex;
    justify-content:space-between;
    align-items:baseline;
    gap:12px;
    flex-wrap:wrap;
  }
  .cv-item-title{
    font-weight:600;
    font-size:14.5px;
    color:var(--cv-ink);
  }
  .cv-item-sub{
    font-size:13px;
    color:var(--cv-muted);
    font-style:italic;
  }
  .cv-item-date{
    font-size:12.5px;
    color:var(--cv-muted);
    white-space:nowrap;
  }
  .cv-item-desc{
    font-size:13.5px;
    color:var(--cv-ink);
    margin:4px 0 0;
    white-space:pre-line;
  }

  .cv-tags{
    display:flex;
    flex-wrap:wrap;
    gap:7px;
  }
  .cv-tag{
    font-size:12.5px;
    border:1px solid var(--cv-rule);
    padding:3px 10px;
    border-radius:2px;
    color:var(--cv-ink);
  }

  .empty-note{
    color:var(--cv-muted);
    font-size:13px;
    font-style:italic;
  }

  /* ===== Print ===== */
  @media print{
    .form-panel{display:none !important;}
    .app{display:block;}
    .preview-panel{background:#fff; padding:0; display:block;}
    .page{box-shadow:none; max-width:none; width:auto; padding:0; min-height:0;}
    body{background:#fff;}
    /* When the fullscreen preview modal is open, print only its content */
    body:has(.preview-modal.is-open) .app{display:none !important;}
  }
</style>
</head>
<body>

<div class="app">

  <!-- ============ FORM PANEL ============ -->
  <div class="form-panel">
    <div class="brand">
      <h1>CV Builder</h1>
      <span>fill in, preview, print</span>
    </div>
    <p class="form-intro">Enter your details on the left. The document on the right updates as you type, and is ready to print or save as a PDF.</p>

    <fieldset>
      <legend>Theme</legend>
      <div class="theme-options" id="themeOptions">
        <button type="button" class="theme-swatch is-active" data-theme="theme-editorial">
          <span class="swatch-color" style="background:#3F6659"></span> Editorial
        </button>
        <button type="button" class="theme-swatch" data-theme="theme-classic">
          <span class="swatch-color" style="background:#8A6D3B"></span> Classic
        </button>
        <button type="button" class="theme-swatch" data-theme="theme-modern">
          <span class="swatch-color" style="background:#2F5CD6"></span> Modern
        </button>
        <button type="button" class="theme-swatch" data-theme="theme-forest">
          <span class="swatch-color" style="background:#4B7A4C"></span> Forest
        </button>
        <button type="button" class="theme-swatch" data-theme="theme-mono">
          <span class="swatch-color" style="background:#111111"></span> Mono
        </button>
      </div>
    </fieldset>

    <fieldset>
      <legend>Basic information</legend>
      <div class="field">
        <label for="fullName">Full name</label>
        <input type="text" id="fullName" placeholder="e.g. Ama Serwaa Boateng">
      </div>
      <div class="field">
        <label for="jobTitle">Professional title</label>
        <input type="text" id="jobTitle" placeholder="e.g. Registered Nurse">
      </div>
      <div class="row2">
        <div class="field">
          <label for="email">Email</label>
          <input type="email" id="email" placeholder="you@example.com">
        </div>
        <div class="field">
          <label for="phone">Phone</label>
          <input type="tel" id="phone" placeholder="+233 ...">
        </div>
      </div>
      <div class="row2">
        <div class="field">
          <label for="location">Location</label>
          <input type="text" id="location" placeholder="City, Country">
        </div>
        <div class="field">
          <label for="website">Website / LinkedIn</label>
          <input type="text" id="website" placeholder="optional">
        </div>
      </div>
      <div class="field">
        <label for="summary">Professional summary</label>
        <textarea id="summary" placeholder="A short paragraph on your experience and strengths."></textarea>
      </div>
    </fieldset>

    <fieldset>
      <legend>Work experience</legend>
      <div id="experienceList"></div>
      <button type="button" class="add-btn" id="addExperience">+ Add work experience</button>
    </fieldset>

    <fieldset>
      <legend>Education</legend>
      <div id="educationList"></div>
      <button type="button" class="add-btn" id="addEducation">+ Add education</button>
    </fieldset>

    <fieldset>
      <legend>Skills</legend>
      <div class="field">
        <label for="skills">Skills (separate with commas)</label>
        <textarea id="skills" placeholder="e.g. Patient triage, Wound care, Team leadership"></textarea>
        <div class="skills-hint">Each item between commas becomes its own tag.</div>
      </div>
    </fieldset>

    <fieldset>
      <legend>Certifications & languages</legend>
      <div class="field">
        <label for="certifications">Certifications (one per line)</label>
        <textarea id="certifications" placeholder="e.g. BLS Certification, 2024"></textarea>
      </div>
      <div class="field">
        <label for="languages">Languages (separate with commas)</label>
        <input type="text" id="languages" placeholder="e.g. English, Twi, French">
      </div>
    </fieldset>

    <div class="action-row">
      <button type="button" class="preview-btn" id="previewBtn">Preview</button>
      <button type="button" class="print-btn" id="printBtn">Print / PDF</button>
    </div>
  </div>

  <!-- ============ PREVIEW PANEL ============ -->
  <div class="preview-panel">
    <div class="page theme-editorial" id="cvPage">
      <!-- populated by JS -->
    </div>
  </div>

</div>

<!-- ============ FULLSCREEN PREVIEW MODAL ============ -->
<div class="preview-modal" id="previewModal">
  <div class="preview-modal-inner">
    <div class="preview-modal-bar">
      <button type="button" class="print-btn-modal" id="modalPrintBtn">Print / PDF</button>
      <button type="button" class="close-btn" id="modalCloseBtn">Close</button>
    </div>
    <div class="page" id="cvPageModal"></div>
  </div>
</div>

<template id="experienceTemplate">
  <div class="entry-block" data-role="experience-entry">
    <button type="button" class="remove-btn" data-action="remove">Remove</button>
    <div class="field">
      <label>Job title</label>
      <input type="text" data-field="role" placeholder="e.g. Staff Nurse">
    </div>
    <div class="field">
      <label>Employer</label>
      <input type="text" data-field="employer" placeholder="e.g. St. Peter Catholic Hospital">
    </div>
    <div class="row2">
      <div class="field">
        <label>Start date</label>
        <input type="text" data-field="start" placeholder="e.g. Jan 2021">
      </div>
      <div class="field">
        <label>End date</label>
        <input type="text" data-field="end" placeholder="e.g. Present">
      </div>
    </div>
    <div class="field">
      <label>Description</label>
      <textarea data-field="desc" placeholder="Key responsibilities and achievements."></textarea>
    </div>
  </div>
</template>

<template id="educationTemplate">
  <div class="entry-block" data-role="education-entry">
    <button type="button" class="remove-btn" data-action="remove">Remove</button>
    <div class="field">
      <label>Qualification</label>
      <input type="text" data-field="degree" placeholder="e.g. BSc Nursing">
    </div>
    <div class="field">
      <label>Institution</label>
      <input type="text" data-field="school" placeholder="e.g. University of Ghana">
    </div>
    <div class="row2">
      <div class="field">
        <label>Start date</label>
        <input type="text" data-field="start" placeholder="e.g. 2014">
      </div>
      <div class="field">
        <label>End date</label>
        <input type="text" data-field="end" placeholder="e.g. 2018">
      </div>
    </div>
  </div>
</template>

<script>
(function(){
  const experienceList = document.getElementById('experienceList');
  const educationList = document.getElementById('educationList');
  const experienceTemplate = document.getElementById('experienceTemplate');
  const educationTemplate = document.getElementById('educationTemplate');
  const cvPage = document.getElementById('cvPage');
  const cvPageModal = document.getElementById('cvPageModal');
  const previewModal = document.getElementById('previewModal');
  const themeButtons = document.querySelectorAll('.theme-swatch');

  function esc(str){
    const div = document.createElement('div');
    div.textContent = str || '';
    return div.innerHTML;
  }

  function addEntry(listEl, templateEl){
    const clone = templateEl.content.cloneNode(true);
    const block = clone.querySelector('[data-role]');
    block.querySelectorAll('input, textarea').forEach(function(el){
      el.addEventListener('input', render);
    });
    block.querySelector('[data-action="remove"]').addEventListener('click', function(){
      block.remove();
      render();
    });
    listEl.appendChild(clone);
    render();
  }

  document.getElementById('addExperience').addEventListener('click', function(){
    addEntry(experienceList, experienceTemplate);
  });
  document.getElementById('addEducation').addEventListener('click', function(){
    addEntry(educationList, educationTemplate);
  });

  document.querySelectorAll('.form-panel input, .form-panel textarea').forEach(function(el){
    el.addEventListener('input', render);
  });

  document.getElementById('printBtn').addEventListener('click', function(){
    window.print();
  });

  document.getElementById('modalPrintBtn').addEventListener('click', function(){
    window.print();
  });

  document.getElementById('previewBtn').addEventListener('click', function(){
    cvPageModal.className = cvPage.className;
    cvPageModal.innerHTML = cvPage.innerHTML;
    previewModal.classList.add('is-open');
    document.body.style.overflow = 'hidden';
  });

  function closeModal(){
    previewModal.classList.remove('is-open');
    document.body.style.overflow = '';
  }
  document.getElementById('modalCloseBtn').addEventListener('click', closeModal);
  previewModal.addEventListener('click', function(e){
    if (e.target === previewModal) closeModal();
  });
  document.addEventListener('keydown', function(e){
    if (e.key === 'Escape' && previewModal.classList.contains('is-open')) closeModal();
  });

  themeButtons.forEach(function(btn){
    btn.addEventListener('click', function(){
      themeButtons.forEach(b => b.classList.remove('is-active'));
      btn.classList.add('is-active');
      cvPage.className = 'page ' + btn.getAttribute('data-theme');
      render();
    });
  });

  function collectEntries(listEl, fields){
    const blocks = listEl.querySelectorAll('[data-role]');
    const out = [];
    blocks.forEach(function(block){
      const item = {};
      fields.forEach(function(f){
        const input = block.querySelector('[data-field="' + f + '"]');
        item[f] = input ? input.value.trim() : '';
      });
      const hasContent = Object.values(item).some(function(v){ return v.length > 0; });
      if (hasContent) out.push(item);
    });
    return out;
  }

  function render(){
    const fullName = document.getElementById('fullName').value.trim();
    const jobTitle = document.getElementById('jobTitle').value.trim();
    const email = document.getElementById('email').value.trim();
    const phone = document.getElementById('phone').value.trim();
    const location = document.getElementById('location').value.trim();
    const website = document.getElementById('website').value.trim();
    const summary = document.getElementById('summary').value.trim();
    const skillsRaw = document.getElementById('skills').value.trim();
    const certsRaw = document.getElementById('certifications').value.trim();
    const languagesRaw = document.getElementById('languages').value.trim();

    const experience = collectEntries(experienceList, ['role','employer','start','end','desc']);
    const education = collectEntries(educationList, ['degree','school','start','end']);
    const skills = skillsRaw ? skillsRaw.split(',').map(s => s.trim()).filter(Boolean) : [];
    const certs = certsRaw ? certsRaw.split('\n').map(s => s.trim()).filter(Boolean) : [];
    const languages = languagesRaw ? languagesRaw.split(',').map(s => s.trim()).filter(Boolean) : [];

    let html = '';

    html += '<div class="cv-name">' + (fullName ? esc(fullName) : 'Your Name') + '</div>';
    if (jobTitle) html += '<div class="cv-title">' + esc(jobTitle) + '</div>';

    const contactBits = [location, phone, email, website].filter(Boolean).map(esc);
    html += '<div class="cv-contact">' + (contactBits.length
      ? contactBits.map(c => '<span>' + c + '</span>').join('<span>&middot;</span>')
      : '<span class="empty-note">Add your contact details on the left</span>') + '</div>';

    if (summary){
      html += '<div class="cv-section"><h2>Summary</h2><p class="cv-summary">' + esc(summary).replace(/\n/g,'<br>') + '</p></div>';
    }

    if (experience.length){
      html += '<div class="cv-section"><h2>Experience</h2>';
      experience.forEach(function(item){
        html += '<div class="cv-item">';
        html += '<div class="cv-item-head">';
        html += '<div><span class="cv-item-title">' + esc(item.role || 'Role') + '</span>';
        if (item.employer) html += '<span class="cv-item-sub"> &mdash; ' + esc(item.employer) + '</span>';
        html += '</div>';
        if (item.start || item.end) html += '<div class="cv-item-date">' + esc(item.start) + (item.start || item.end ? ' \u2013 ' : '') + esc(item.end) + '</div>';
        html += '</div>';
        if (item.desc) html += '<p class="cv-item-desc">' + esc(item.desc) + '</p>';
        html += '</div>';
      });
      html += '</div>';
    }

    if (education.length){
      html += '<div class="cv-section"><h2>Education</h2>';
      education.forEach(function(item){
        html += '<div class="cv-item">';
        html += '<div class="cv-item-head">';
        html += '<div><span class="cv-item-title">' + esc(item.degree || 'Qualification') + '</span>';
        if (item.school) html += '<span class="cv-item-sub"> &mdash; ' + esc(item.school) + '</span>';
        html += '</div>';
        if (item.start || item.end) html += '<div class="cv-item-date">' + esc(item.start) + (item.start || item.end ? ' \u2013 ' : '') + esc(item.end) + '</div>';
        html += '</div>';
        html += '</div>';
      });
      html += '</div>';
    }

    if (skills.length){
      html += '<div class="cv-section"><h2>Skills</h2><div class="cv-tags">' +
        skills.map(s => '<span class="cv-tag">' + esc(s) + '</span>').join('') +
        '</div></div>';
    }

    if (certs.length){
      html += '<div class="cv-section"><h2>Certifications</h2><div class="cv-tags">' +
        certs.map(s => '<span class="cv-tag">' + esc(s) + '</span>').join('') +
        '</div></div>';
    }

    if (languages.length){
      html += '<div class="cv-section"><h2>Languages</h2><div class="cv-tags">' +
        languages.map(s => '<span class="cv-tag">' + esc(s) + '</span>').join('') +
        '</div></div>';
    }

    cvPage.innerHTML = html;
  }

  // Start with one experience and one education block ready to fill in
  addEntry(experienceList, experienceTemplate);
  addEntry(educationList, educationTemplate);
  render();
})();
</script>

</body>
</html>
