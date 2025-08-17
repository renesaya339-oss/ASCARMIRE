# ASCARMIRE
# Build a complete multi-page website for ASC ARMIRE and package it for download
import os, zipfile, json, textwrap, datetime, pathlib

base = "/mnt/data/asc-armire-site-full"
assets = os.path.join(base, "assets")
os.makedirs(assets, exist_ok=True)

year = 2025

# Shared color palette
palette = {
    "blue": "#0A62FF",
    "green": "#00A86B",
    "white": "#FFFFFF",
    "dark": "#0F172A",
    "light": "#F2F7FF"
}

# Content JSON for easy editing
content = {
    "club": {
        "name": "ASC ARMIRE",
        "tagline": "Le club qui fait vibrer le terrain",
        "city": "Cayenne, Guyane française",
        "email": "contact@ascarmire.com",
        "address": "Stade Municipal, Cayenne",
        "socials": {
            "instagram": "https://instagram.com/ascarmire",
            "facebook": "https://facebook.com/ascarmire"
        }
    },
    "news": [
        {"title": "Recrutement U17 ouvert", "date": "2025-09-01", "tag": "Jeunes", "excerpt": "Inscris-toi aux séances d'essai pour rejoindre l'académie U17.", "link": "#"},
        {"title": "Victoire 3–1 au derby", "date": "2025-08-10", "tag": "Match", "excerpt": "Belle performance collective et ambiance de feu au stade municipal.", "link": "#"},
        {"title": "Nouveau partenaire local", "date": "2025-08-02", "tag": "Club", "excerpt": "Bienvenue à notre sponsor officiel : Armire Solutions.", "link": "#"}
    ],
    "fixtures": [
        {"date": "2025-08-20", "comp": "Régional 1", "home": "ASC ARMIRE", "score": "—", "away": "US Kourou", "venue": "Stade Municipal", "type": "home"},
        {"date": "2025-08-28", "comp": "Coupe Locale", "home": "CS Matoury", "score": "—", "away": "ASC ARMIRE", "venue": "Stade de Matoury", "type": "away"},
        {"date": "2025-09-05", "comp": "Régional 1", "home": "ASC ARMIRE", "score": "—", "away": "ASC Remire", "venue": "Stade Municipal", "type": "home"}
    ],
    "team": [
        {"number": 1, "name": "D. Laurent", "role": "Gardien"},
        {"number": 3, "name": "M. Silva", "role": "Défenseur"},
        {"number": 5, "name": "K. Diallo", "role": "Défenseur"},
        {"number": 7, "name": "A. Nascimento", "role": "Milieu"},
        {"number": 9, "name": "Y. Mendes", "role": "Attaquant"},
        {"number": 10, "name": "S. Morel", "role": "Milieu"},
        {"number": 11, "name": "N. B. Soumah", "role": "Attaquant"},
        {"number": 14, "name": "T. Césaire", "role": "Milieu"}
    ],
    "gallery": [
        {"src": "assets/ph1.svg", "caption": "Jour de match"},
        {"src": "assets/ph2.svg", "caption": "Victoire !"},
        {"src": "assets/ph3.svg", "caption": "Le 12e homme"},
        {"src": "assets/ph4.svg", "caption": "À l'entraînement"}
    ]
}

# Save content.json
with open(os.path.join(base, "content.json"), "w", encoding="utf-8") as f:
    json.dump(content, f, ensure_ascii=False, indent=2)

# Simple SVG placeholders for gallery
placeholder_svg = lambda label: f"""<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1200 800">
  <defs>
    <linearGradient id="g" x1="0" x2="1">
      <stop offset="0%" stop-color="{palette['blue']}"/>
      <stop offset="100%" stop-color="{palette['green']}"/>
    </linearGradient>
  </defs>
  <rect width="1200" height="800" fill="url(#g)"/>
  <g fill="#ffffff" opacity="0.9">
    <circle cx="260" cy="260" r="80"/>
    <rect x="380" y="200" width="500" height="120" rx="20"/>
    <rect x="380" y="360" width="420" height="80" rx="16" opacity="0.8"/>
  </g>
  <text x="600" y="620" text-anchor="middle" font-family="Poppins, Arial" font-size="64" fill="#ffffff" font-weight="800">{label}</text>
</svg>"""
for i, label in enumerate(["ASC ARMIRE", "Victoire", "Supporters", "Entraînement"], start=1):
    with open(os.path.join(assets, f"ph{i}.svg"), "w", encoding="utf-8") as f:
        f.write(placeholder_svg(label))

# Logo SVG
logo_svg = f"""<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 220 260">
  <defs>
    <linearGradient id="g1" x1="0" x2="1">
      <stop offset="0%" stop-color="{palette['blue']}"/>
      <stop offset="100%" stop-color="{palette['green']}"/>
    </linearGradient>
  </defs>
  <path d="M110 10 L200 50 L190 160 C185 190 150 220 110 240 C70 220 35 190 30 160 L20 50 Z" fill="url(#g1)" stroke="{palette['dark']}" stroke-width="4" />
  <circle cx="110" cy="110" r="44" fill="#fff" stroke="{palette['dark']}" stroke-width="4"/>
  <path d="M110 72 l18 14 -7 21 h-22 l-7 -21 z" fill="{palette['dark']}"/>
  <text x="110" y="198" text-anchor="middle" font-size="28" font-family="Poppins, Arial" fill="#fff" font-weight="800">ASC</text>
  <text x="110" y="224" text-anchor="middle" font-size="18" font-family="Poppins, Arial" fill="#fff">ARMIRE</text>
</svg>"""
with open(os.path.join(assets, "logo.svg"), "w", encoding="utf-8") as f:
    f.write(logo_svg)

# Favicon (small svg)
with open(os.path.join(base, "favicon.svg"), "w", encoding="utf-8") as f:
    f.write(logo_svg)

# Shared head and header/footer templates
head_common = f"""
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;800&display=swap" rel="stylesheet">
  <link rel="icon" type="image/svg+xml" href="favicon.svg" />
  <link rel="stylesheet" href="style.css" />
"""

header_nav = """
  <header class="site-header">
    <div class="brand">
      <img src="assets/logo.svg" alt="Logo ASC ARMIRE" class="logo" />
      <div class="brand-text">
        <h1>ASC ARMIRE</h1>
        <p>Le club qui fait vibrer le terrain</p>
      </div>
    </div>
    <nav class="nav">
      <a href="index.html">Accueil</a>
      <a href="actus.html">Actualités</a>
      <a href="matchs.html">Matchs</a>
      <a href="equipe.html">Équipe</a>
      <a href="galerie.html">Galerie</a>
      <a href="contact.html">Contact</a>
    </nav>
  </header>
"""

footer_common = f"""
  <footer class="site-footer">
    <p>© {year} ASC ARMIRE — Tous droits réservés.</p>
  </footer>
  <script src="site.js"></script>
"""

# Pages HTML
pages = {
"index.html": f"""<!DOCTYPE html>
<html lang="fr">
<head>
  <title>ASC ARMIRE — Accueil</title>
  <meta name="description" content="Site officiel de l'ASC ARMIRE : actualités, résultats, équipe et partenariats." />
{head_common}
</head>
<body>
{header_nav}
  <main>
    <section class="hero">
      <div class="hero-content">
        <h2><span>Force</span>, <span>Esprit d'équipe</span>, <span>Victoire</span>.</h2>
        <p>Bienvenue sur le site officiel de l'ASC ARMIRE. Suivez nos résultats, découvrez nos joueurs et revivez les meilleurs moments du club.</p>
        <div class="cta">
          <a class="btn primary" href="matchs.html">Prochains matchs</a>
          <a class="btn ghost" href="contact.html">Devenir partenaire</a>
        </div>
      </div>
      <svg class="hero-wave" viewBox="0 0 1440 150" preserveAspectRatio="none">
        <path d="M0,64L80,96C160,128,320,192,480,202.7C640,213,800,171,960,133.3C1120,96,1280,64,1360,48L1440,32L1440,0L1360,0C1280,0,1120,0,960,0C800,0,640,0,480,0C320,0,160,0,80,0L0,0Z"></path>
      </svg>
    </section>
    <section class="section">
      <h3 class="section-title">📢 Dernières actualités</h3>
      <div class="cards" data-mount="news-home"></div>
      <div class="center mt-1"><a class="btn tiny" href="actus.html">Voir toutes les actus</a></div>
    </section>
  </main>
{footer_common}
</body>
</html>
""",
"actus.html": f"""<!DOCTYPE html>
<html lang="fr">
<head>
  <title>ASC ARMIRE — Actualités</title>
  <meta name="description" content="Les actualités officielles de l'ASC ARMIRE : annonces, résultats, vie du club." />
{head_common}
</head>
<body>
{header_nav}
  <main>
    <section class="section">
      <h3 class="section-title">📢 Actualités</h3>
      <div class="cards" data-mount="news-list"></div>
    </section>
  </main>
{footer_common}
</body>
</html>
""",
"matchs.html": f"""<!DOCTYPE html>
<html lang="fr">
<head>
  <title>ASC ARMIRE — Matchs</title>
  <meta name="description" content="Calendrier des matchs de l'ASC ARMIRE : domicile et extérieur." />
{head_common}
</head>
<body>
{header_nav}
  <main>
    <section class="section">
      <h3 class="section-title">📅 Matchs</h3>
      <div class="match-filters">
        <button class="btn tiny" data-filter="all">Tous</button>
        <button class="btn tiny" data-filter="home">Domicile</button>
        <button class="btn tiny" data-filter="away">Extérieur</button>
      </div>
      <div class="table-wrap">
        <table class="table" aria-describedby="matchs">
          <thead>
            <tr>
              <th>Date</th>
              <th>Compétition</th>
              <th>Domicile</th>
              <th>Score</th>
              <th>Extérieur</th>
              <th>Lieu</th>
            </tr>
          </thead>
          <tbody data-mount="fixtures"></tbody>
        </table>
      </div>
    </section>
  </main>
{footer_common}
</body>
</html>
""",
"equipe.html": f"""<!DOCTYPE html>
<html lang="fr">
<head>
  <title>ASC ARMIRE — Équipe</title>
  <meta name="description" content="Effectif de l'ASC ARMIRE : postes, numéros et joueurs." />
{head_common}
</head>
<body>
{header_nav}
  <main>
    <section class="section">
      <h3 class="section-title">👥 Équipe première</h3>
      <div class="grid team" data-mount="team"></div>
    </section>
  </main>
{footer_common}
</body>
</html>
""",
"galerie.html": f"""<!DOCTYPE html>
<html lang="fr">
<head>
  <title>ASC ARMIRE — Galerie</title>
  <meta name="description" content="Photos officielles de l'ASC ARMIRE : matchs, entraînements, supporters." />
{head_common}
</head>
<body>
{header_nav}
  <main>
    <section class="section">
      <h3 class="section-title">📸 Galerie</h3>
      <div class="grid gallery" data-mount="gallery"></div>
    </section>
  </main>
{footer_common}
</body>
</html>
""",
"contact.html": f"""<!DOCTYPE html>
<html lang="fr">
<head>
  <title>ASC ARMIRE — Contact</title>
  <meta name="description" content="Contacter l'ASC ARMIRE : adresse, email, réseaux sociaux, partenariat." />
{head_common}
</head>
<body>
{header_nav}
  <main>
    <section class="section">
      <h3 class="section-title">📬 Contact</h3>
      <div class="contact-wrap">
        <form class="card contact-form" name="contact" method="POST" data-netlify="true">
          <input type="hidden" name="form-name" value="contact" />
          <label>Nom
            <input type="text" name="name" placeholder="Votre nom" required />
          </label>
          <label>Email
            <input type="email" name="email" placeholder="votre@email.com" required />
          </label>
          <label>Message
            <textarea name="message" rows="5" placeholder="Votre message..."></textarea>
          </label>
          <button class="btn primary" type="submit">Envoyer</button>
          <p class="form-note">Hébergé sur Netlify ? Ce formulaire est déjà prêt.</p>
        </form>

        <div class="card club-info" data-mount="club-info"></div>
      </div>
    </section>
  </main>
{footer_common}
</body>
</html>
"""
}

# Write pages
for filename, html in pages.items():
    with open(os.path.join(base, filename), "w", encoding="utf-8") as f:
        f.write(html)

# Stylesheet
style_css = f"""/* === ASC ARMIRE Styles (multi-page) === */
:root {{
  --blue: {palette['blue']};
  --green: {palette['green']};
  --white: {palette['white']};
  --dark: {palette['dark']};
  --light: {palette['light']};
  --radius: 16px;
  --shadow: 0 10px 30px rgba(0,0,0,.12);
}}

* {{ box-sizing: border-box; }}
html, body {{ margin: 0; padding: 0; }}
body {{
  font-family: 'Poppins', system-ui, -apple-system, Segoe UI, Roboto, sans-serif;
  color: var(--dark);
  background: linear-gradient(180deg, var(--light), #fff);
}}

.site-header {{
  position: sticky; top: 0; z-index: 50;
  display: grid; grid-template-columns: 1fr auto; align-items: center;
  gap: 16px;
  padding: 14px min(6vw, 32px);
  background: linear-gradient(90deg, var(--blue), var(--green));
  color: var(--white);
  box-shadow: var(--shadow);
}}

.brand {{ display: flex; align-items: center; gap: 12px; }}
.brand .logo {{ width: 52px; height: 52px; }}
.brand-text h1 {{ margin: 0; font-size: clamp(18px, 2.4vw, 26px); line-height: 1; }}
.brand-text p {{ margin: 2px 0 0; opacity: .9; font-weight: 300; font-size: 12px; }}

.nav a {{
  color: var(--white); text-decoration: none; font-weight: 600;
  padding: 10px 14px; border-radius: 999px; display: inline-block;
  transition: transform .15s ease, background .2s ease;
}}
.nav a:hover {{ background: rgba(255,255,255,.2); transform: translateY(-1px); }}

.hero {{
  position: relative; isolation: isolate;
  padding: min(10vh, 120px) min(6vw, 32px) 60px;
  background:
    radial-gradient(1200px 300px at 10% -10%, rgba(255,255,255,.3), transparent 60%),
    linear-gradient(135deg, var(--blue), var(--green));
  color: var(--white);
  overflow: hidden;
}}
.hero-content {{ max-width: 1100px; margin: 0 auto; }}
.hero h2 {{ font-size: clamp(28px, 6vw, 56px); margin: 0 0 10px; line-height: 1.05; }}
.hero h2 span {{ background: rgba(255,255,255,.18); padding: .1em .25em; border-radius: .35em; }}
.hero p {{ font-size: clamp(14px, 2.5vw, 20px); max-width: 70ch; opacity: .95; }}
.cta {{ display: flex; gap: 12px; margin-top: 18px; flex-wrap: wrap; }}

.hero-wave {{ position: absolute; bottom: -1px; left: 0; width: 100%; height: 110px; fill: #fff; }}

.section {{ padding: 56px min(6vw, 32px); }}
.section-title {{ text-align: center; margin: 0 0 26px; font-size: clamp(22px, 4vw, 32px); color: var(--blue); }}

.cards {{ display: grid; grid-template-columns: repeat(auto-fit, minmax(260px, 1fr)); gap: 18px; }}
.card {{
  background: rgba(255,255,255,.85);
  backdrop-filter: blur(8px);
  border-radius: var(--radius);
  box-shadow: var(--shadow);
  padding: 18px;
  border: 1px solid rgba(10,98,255,.08);
}}
.card .meta {{ display: flex; align-items: center; gap: 10px; font-size: 12px; opacity: .8; }}

.table-wrap {{ overflow-x: auto; border-radius: var(--radius); box-shadow: var(--shadow); }}
.table {{ width: 100%; border-collapse: collapse; background: #fff; }}
.table th, .table td {{ padding: 12px 14px; border-bottom: 1px solid #eef3ff; text-align: left; }}
.table thead th {{ background: linear-gradient(90deg, var(--blue), var(--green)); color: #fff; position: sticky; top: 0; }}
.table tbody tr:hover {{ background: #f6fbff; }}

.grid {{ display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); gap: 18px; }}
.team .player {{ display: grid; grid-template-rows: 140px auto; gap: 10px; }}
.player .avatar {{ background: linear-gradient(135deg, var(--blue), var(--green)); border-radius: 14px; display: grid; place-items: center; color: #fff; font-weight: 800; font-size: 46px; }}
.player h4 {{ margin: 0; font-size: 18px; }}
.player p {{ margin: 4px 0 0; opacity: .8; font-size: 14px; }}

.gallery .item {{ overflow: hidden; border-radius: 14px; box-shadow: var(--shadow); background: #fff; }}
.gallery .item img {{ width: 100%; display: block; }}
.gallery .item figcaption {{ padding: 10px 12px; font-size: 13px; }}

.contact-wrap {{ display: grid; grid-template-columns: 1.2fr .8fr; gap: 18px; }}
.contact-form label {{ display: grid; gap: 6px; font-size: 14px; }}
input, textarea {{
  width: 100%; padding: 12px 14px; border-radius: 10px; border: 1px solid #e2e8ff; background: #fff; font: inherit;
}}
input:focus, textarea:focus {{ outline: 2px solid rgba(10,98,255,.25); border-color: var(--blue); }}

.badges {{ display: flex; flex-wrap: wrap; gap: 8px; margin-top: 10px; }}
.badge {{ background: linear-gradient(90deg, var(--blue), var(--green)); color: #fff; padding: 6px 10px; border-radius: 999px; font-size: 12px; }}

.btn {{ border: 0; display: inline-flex; align-items: center; justify-content: center; padding: 10px 16px; border-radius: 12px; font-weight: 700; cursor: pointer; text-decoration: none; }}
.btn.primary {{ background: linear-gradient(90deg, var(--blue), var(--green)); color: #fff; }}
.btn.ghost {{ background: rgba(255,255,255,.2); color: #fff; border: 1px solid rgba(255,255,255,.45); }}
.btn.tiny {{ padding: 8px 12px; border-radius: 999px; background: #e8f1ff; color: var(--blue); }}
.btn.tiny:hover {{ filter: brightness(.95); }}

.center {{ text-align: center; }}
.mt-1 {{ margin-top: 12px; }}

.site-footer {{ text-align: center; padding: 30px 16px; color: #5b677f; }}

@media (max-width: 860px) {{
  .site-header {{ grid-template-columns: 1fr; }}
  .nav {{ display: flex; overflow-x: auto; gap: 6px; }}
  .contact-wrap {{ grid-template-columns: 1fr; }}
}}
"""
with open(os.path.join(base, "style.css"), "w", encoding="utf-8") as f:
    f.write(style_css)

# Site JS to load content.json
site_js = r"""// Load JSON content and mount into pages
(async function(){
  const res = await fetch('content.json');
  const data = await res.json();

  // Home recent news
  const newsHome = document.querySelector('[data-mount="news-home"]');
  if (newsHome) {
    const latest = [...data.news].sort((a,b)=> new Date(b.date) - new Date(a.date)).slice(0,3);
    latest.forEach(n => newsHome.appendChild(newsCard(n)));
  }

  // All news
  const newsList = document.querySelector('[data-mount="news-list"]');
  if (newsList) {
    const all = [...data.news].sort((a,b)=> new Date(b.date) - new Date(a.date));
    all.forEach(n => newsList.appendChild(newsCard(n)));
  }

  // Fixtures
  const fixturesBody = document.querySelector('[data-mount="fixtures"]');
  const filtersWrap = document.querySelector('.match-filters');
  if (fixturesBody) {
    const render = (filter='all') => {
      fixturesBody.innerHTML = '';
      data.fixtures
        .filter(f => filter==='all' || f.type===filter)
        .forEach(f => fixturesBody.appendChild(trFixture(f)));
    };
    render();
    if (filtersWrap) {
      filtersWrap.querySelectorAll('button').forEach(btn => {
        btn.addEventListener('click', () => render(btn.dataset.filter));
      });
    }
  }

  // Team
  const teamGrid = document.querySelector('[data-mount="team"]');
  if (teamGrid) {
    data.team.forEach(p => {
      const card = document.createElement('div');
      card.className = 'card player';
      card.innerHTML = `
        <div class="avatar">${p.number}</div>
        <div>
          <h4>${p.name}</h4>
          <p>${p.role}</p>
        </div>
      `;
      teamGrid.appendChild(card);
    });
  }

  // Gallery
  const gal = document.querySelector('[data-mount="gallery"]');
  if (gal) {
    data.gallery.forEach(g => {
      const fig = document.createElement('figure');
      fig.className = 'item';
      fig.innerHTML = <img src="${g.src}" alt="${g.caption}"><figcaption>${g.caption}</figcaption>;
      gal.appendChild(fig);
    });
  }

  // Club info
  const clubInfo = document.querySelector('[data-mount="club-info"]');
  if (clubInfo) {
    const c = data.club;
    clubInfo.innerHTML = `
      <h4>${c.name}</h4>
      <ul>
        <li><strong>Adresse :</strong> ${c.address}</li>
        <li><strong>Email :</strong> <a href="mailto:${c.email}">${c.email}</a></li>
        <li><strong>Ville :</strong> ${c.city}</li>
        <li><strong>Réseaux :</strong> 
          <a href="${c.socials.instagram}" target="_blank" rel="noopener">Instagram</a> · 
          <a href="${c.socials.facebook}" target="_blank" rel="noopener">Facebook</a>
        </li>
      </ul>
      <div class="badges">
        <span class="badge">Bleu</span>
        <span class="badge">Blanc</span>
        <span class="badge">Vert</span>
      </div>
    `;
  }

  // Helpers
  function newsCard(n){
    const el = document.createElement('article');
    el.className = 'card';
    el.innerHTML = `
      <div class="meta">
        <span>${new Date(n.date).toLocaleDateString('fr-FR')}</span>
        <span>•</span>
        <span>${n.tag}</span>
      </div>
      <h4>${n.title}</h4>
      <p>${n.excerpt}</p>
      <a class="btn tiny" href="${n.link}">Lire</a>
    `;
    return el;
  }
  function trFixture(f){
    const tr = document.createElement('tr');
    tr.innerHTML = `
      <td>${new Date(f.date).toLocaleDateString('fr-FR')}</td>
      <td>${f.comp}</td>
      <td>${f.home}</td>
      <td>${f.score}</td>
      <td>${f.away}</td>
      <td>${f.venue}</td>
    `;
    return tr;
  }
})();"""
with open(os.path.join(base, "site.js"), "w", encoding="utf-8") as f:
    f.write(site_js)

# README
readme = f"""# ASC ARMIRE — Site web complet (multi-pages)

Ce pack contient un site *clé en main* pour l'ASC ARMIRE en *bleu / blanc / vert*.

## 📦 Structure
- index.html — Accueil (hero + dernières actus)
- actus.html — Liste des actualités
- matchs.html — Calendrier (filtres domicile/extérieur)
- equipe.html — Équipe (carte joueur)
- galerie.html — Galerie photo
- contact.html — Contact (formulaire Netlify prêt)
- content.json — *Contenu éditable* (actus, matchs, équipe, galerie, infos club)
- style.css — Styles responsive (dégradés, vagues, glassmorphism)
- site.js — Charge content.json et injecte le contenu
- assets/logo.svg, assets/ph*.svg — Logo + images de placeholder
- favicon.svg

## ✏ Modifier le contenu
Ouvre content.json et change :
- club (nom, email, adresse, réseaux)
- news, fixtures, team, gallery

Enregistre → recharge la page.

## 🚀 Mise en ligne
*Option A — Netlify (recommandé)*  
1. Crée un compte Netlify (gratuit)  
2. Drag & drop le *dossier complet* ici.  
3. Le formulaire contact fonctionne sans code serveur (grâce à data-netlify="true").

*Option B — GitHub Pages*  
- Pousse les fichiers dans un dépôt → Settings → Pages → Source: main / root → Save

*Option C — Hébergeur / FTP*  
- Envoie tout dans le dossier www/

## 🧩 Aller plus loin
- Admin sans code: connecter content.json à un CMS headless (Netlify CMS, Decap)  
- Pages supplémentaires: Jeunes, Partenaires, Boutique (Stripe)  
- SEO: titres/descr. par page (déjà en place), sitemap

Bon match !
"""
with open(os.path.join(base, "README.md"), "w", encoding="utf-8") as f:
    f.write(readme)

# Zip the folder
zip_path = "/mnt/data/ASC_ARMIRE_site_complet.zip"
with zipfile.ZipFile(zip_path, "w", zipfile.ZIP_DEFLATED) as z:
    for folder, _, filenames in os.walk(base):
        for fn in filenames:
            full = os.path.join(folder, fn)
            z.write(full, arcname=os.path.relpath(full, base))

zip_path
:root{
  --violet:#7b5cff;    /* couleurs principales */
  --pastel-violet:#bfa8ff;
  --gold:#c8a54a;
  --white:#ffffff;
  --muted:#6b6b6b;
  --bg:#faf8ff;
  --container:1100px;
  font-family: "Nunito", system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
}

*{box-sizing:border-box}
body{margin:0;background:var(--bg);color:#111;line-height:1.45}
.container{max-width:var(--container);margin:0 auto;padding:1rem}

.site-header{
  background:linear-gradient(90deg,var(--violet),var(--pastel-violet));
  color:var(--white);
  padding:0.5rem 0;
  position:sticky;top:0;z-index:40;
  box-shadow:0 4px 12px rgba(0,0,0,0.08);
}
.header-inner{display:flex;align-items:center;justify-content:space-between;gap:1rem}
.brand{display:flex;align-items:center;gap:.6rem;color:inherit;text-decoration:none}
.logo{height:44px;width:44px;object-fit:cover;border-radius:6px;background:var(--white)}
.club-name{font-weight:800;letter-spacing:.6px}

#nav-toggle{display:none;background:transparent;border:0;color:var(--white);font-size:20px}
.nav ul{display:flex;gap:1rem;list-style:none;margin:0;padding:0}
.nav a{color:var(--white);text-decoration:none;padding:.5rem .6rem;border-radius:6px;font-weight:600}
.nav a:hover{background:rgba(255,255,255,0.08)}

.hero{padding:3rem 0}
.hero-inner{display:grid;grid-template-columns:1fr 420px;gap:2rem;align-items:center}
.hero h1{font-size:2.1rem;margin:0}
.lead{color:var(--muted);margin:.6rem 0 1.2rem}
.hero-image img{width:100%;border-radius:12px;box-shadow:0 8px 30px rgba(0,0,0,0.12)}

.btn{display:inline-block;padding:.6rem 1rem;border-radius:8px;background:transparent;border:2px solid rgba(0,0,0,0.06);text-decoration:none;font-weight:700}
.btn-primary{background:linear-gradient(90deg,var(--gold),var(--violet));color:#fff;border:0}
.hero-cta .btn{margin-right:.6rem}

.quick-info{display:flex;gap:1rem;margin-top:-1.6rem;position:relative;z-index:10}
.card{flex:1;background:#fff;padding:1rem;border-radius:10px;box-shadow:0 8px 20px rgba(11,12,15,0.04)}
.card h3{margin:0 0 .4rem}

.section{padding:2rem 0}
.players-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(170px,1fr));gap:1rem}
.player{
  background:#fff;border-radius:10px;padding:1rem;text-align:center;box-shadow:0 8px 30px rgba(0,0,0,0.06);
  cursor:pointer;transition:transform .15s ease;
}
.player:hover{transform:translateY(-6px)}
.player img{width:96px;height:96px;border-radius:50%;object-fit:cover;margin-bottom:.6rem}

.table{width:100%;border-collapse:collapse;background:#fff;border-radius:8px;overflow:hidden;box-shadow:0 8px 20px rgba(0,0,0,0.04)}
.table th,.table td{padding:.75rem 1rem;text-align:left;border-bottom:1px solid #f1f1f1}
.table thead{background:linear-gradient(90deg,var(--pastel-violet),#efe8ff);font-weight:700}

.results-list{list-style:none;padding:0;margin:0}
.results-list li{background:#fff;padding:.8rem;border-radius:8px;margin-bottom:.6rem;box-shadow:0 6px 18px rgba(0,0,0,0.04)}

.gallery-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(150px,1fr));gap:.6rem}
.gallery-grid img{width:100%;height:160px;object-fit:cover;border-radius:8px}

.contact-grid{display:grid;grid-template-columns:1fr 320px;gap:1rem}
.contact-form{background:#fff;padding:1rem;border-radius:10px;box-shadow:0 8px 20px rgba(0,0,0,0.04)}
.contact-form label{display:block;margin-bottom:.7rem;font-weight:600}
.contact-form input,.contact-form textarea{width:100%;padding:.6rem;border-radius:6px;border:1px solid #ddd;margin-top:.25rem}
.contact-info{background:#fff;padding:1rem;border-radius:10px;box-shadow:0 8px 20px rgba(0,0,0,0.04)}

.site-footer{padding:1rem 0;text-align:center;color:var(--muted)}

.muted{color:var(--muted)}
.small{font-size:.85rem}

/* responsive */
@media (max-width:900px){
  .hero-inner{grid-template-columns:1fr}
  #nav-toggle{display:block}
  .nav{position:absolute;top:64px;right:10px;background:rgba(255,255,255,0.98);padding:1rem;border-radius:10px;display:none;box-shadow:0 14px 40px rgba(0,0,0,0.12)}
  .nav.open{display:block}
  .nav ul{flex-direction:column}
  .quick-info{flex-direction:column}
  .contact-grid{grid-template-columns:1fr}
}
