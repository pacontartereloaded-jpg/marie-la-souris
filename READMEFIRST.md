# READMEFIRST — Marie la Souris · Contexte IA

> **Lis ce fichier avant toute modification.** Il est mis à jour automatiquement à chaque changement.
> Last updated: 2026-04-16

---

## Projet

**Nom:** Marie la Souris  
**Type:** Landing page statique (HTML/CSS/JS — fichier unique `index.html`)  
**Langue principale:** Français (i18n FR/ES/EN intégré)  
**Déployé:** GitHub (privé) → Vercel (auto-deploy sur push)  
**GitHub:** https://github.com/pacontartereloaded-jpg/marie-la-souris (branche `master`)  
**Email propriétaire:** cuentasdelol100@gmail.com

---

## Structure des fichiers

```
E:\Pagina de Cote\
├── index.html              ← PAGE UNIQUE — tout HTML/CSS/JS ici
├── READMEFIRST.md          ← ce fichier
├── .gitignore
├── mice/                   ← 16 PNG transparents (ratoncitos peekers)
│   ├── mouse-01.png … mouse-16.png
├── img/
│   ├── hero-basket.jpg
│   ├── product-cartes.jpg
│   ├── product-emotions.jpg
│   ├── product-memory.jpg
│   ├── newsletter-img.jpg
│   ├── mouse-hero-nobg.png  ← grande souris héro (droite du hero)
│   ├── mouse-logo-nobg.png  ← petite souris nav
│   └── botanical-branch.png
└── generate_images.py      ← scripts Python pour régénérer les mice (nano-banana API)
```

---

## Architecture `index.html`

### Sections (dans l'ordre)
| ID | Titre | Contenu |
|----|-------|---------|
| `#inicio` | Hero | Image panier + texte + hero dots (3 slides) + mouse décorative |
| *(tagline)* | Tagline | Bandeau animé + 3 valeurs |
| `#productos` | Boutique | 3 cartes produit ($4.99, $6.99, $7.99) |
| `#historia` | Notre histoire / Sobre mí | Texte biographie + 3 cartes valeurs |
| `#faq` | FAQ | 6 questions accordion |
| `#contacto` | Newsletter | Formulaire email |
| *(footer)* | Footer | 3 colonnes de liens |

### Navigation
- 3 colonnes: liens gauche | logo+mouse | recherche+langue+icônes
- Liens fonctionnels: Accueil→`#inicio`, Boutique/Nouveautés/Imprimables→`#productos`, Éducatif→`#historia`, Blog→`#contacto`
- Footer: liens mappés aux sections correspondantes

### i18n (multilingue)
- Système `data-i18n` + objet JS `T` avec clés `fr`/`es`/`en`
- Fonction `setLang(l)` applique les traductions
- Boutons langue: FR | ES | EN dans la nav

---

## Système des ratoncitos peekers

### Concept
4 coins de l'écran ont des souris qui se glissent puis disparaissent :
- `#pL` — bord gauche (mid-height)
- `#pR` — bord droit (mid-height)
- `#pBL` — bord bas gauche
- `#pBR` — bord bas droit

> Le peeker du haut (`#pT`) a été **supprimé** car il apparaissait toujours coupé.

### Array MICE (JS, ~ligne 1490)
```js
const MICE = [
  { f:'mice/mouse-01.png', dir:'f', ok:['pL','pR','pBL','pBR'] },
  { f:'mice/mouse-02.png', dir:'r', ok:['pL','pR','pBL','pBR'] },
  { f:'mice/mouse-03.png', dir:'l', ok:['pL','pR','pBL','pBR'] },
  { f:'mice/mouse-04.png', dir:'r', ok:['pL','pR','pBL','pBR'] },
  { f:'mice/mouse-05.png', dir:'l', ok:['pL','pR','pBL','pBR'] },
  { f:'mice/mouse-06.png', dir:'r', ok:['pL','pR','pBL','pBR'] },
  { f:'mice/mouse-07.png', dir:'r', ok:['pL','pR','pBL','pBR'] },
  { f:'mice/mouse-08.png', dir:'r', ok:['pL','pR','pBL','pBR'] },
  { f:'mice/mouse-09.png', dir:'r', ok:['pL','pR','pBL','pBR'] },
  { f:'mice/mouse-10.png', dir:'r', ok:['pL']                  }, // seulement gauche
  { f:'mice/mouse-11.png', dir:'f', ok:['pL','pR','pBL','pBR'] },
  { f:'mice/mouse-12.png', dir:'l', ok:['pL','pR','pBL','pBR'] },
  { f:'mice/mouse-13.png', dir:'l', ok:['pL','pR','pBL','pBR'] },
  { f:'mice/mouse-14.png', dir:'f', ok:['pL','pR','pBL','pBR'] },
  { f:'mice/mouse-15.png', dir:'f', ok:['pBL','pBR']           }, // seulement bas
  { f:'mice/mouse-16.png', dir:'r', ok:['pL','pR','pBL','pBR'] },
];
```

### Logique d'orientation
- `dir:'r'` = souris face droite → flip si position `pR` (pour regarder dans l'écran)
- `dir:'l'` = souris face gauche → flip si position `pL`
- `dir:'f'` = symétrique (face) → jamais flip
- `shouldFlip(id, dir)` applique `scaleX(-1)` sur l'`<img>` (PAS le conteneur)

### CSS peeker (clé)
```css
#pL  { left:0; top:38%; width:195px; transform:translateX(-200px); }
#pL.show { transform:translateX(-20px); }
#pR  { right:0; top:52%; width:195px; transform:translateX(200px); }
#pR.show { transform:translateX(20px); }
#pBL { left:12%; bottom:0; width:180px; transform:translateY(100%); }
#pBL.show { transform:translateY(25%); }
#pBR { right:16%; bottom:0; width:180px; transform:translateY(100%); }
#pBR.show { transform:translateY(25%); }
```

---

## Style des images (mice)

**Style approuvé:** Flat vector, taupe/rose, SANS contours, SANS ombres portées.

**Palette exacte:**
- Corps: `#C3A38B`
- Oreilles/pattes: `#E5B5AE`
- Taches: `#A38977`
- Yeux: `#49362E`

**API de génération:** nano-banana-pro-preview (Gemini 3 Pro Image)  
**Suppression fond:** `rembg` Python  
**Script:** `generate_images.py` (+ scripts `regen_*.py` dans le dossier)

---

## Hero Dots (3 slides)

Système JS (~ligne 1415) avec auto-advance 6 secondes :
1. Slide 0 — Hero principal (découvrir les produits → `#productos`)
2. Slide 1 — Notre histoire (en savoir plus → `#historia`)
3. Slide 2 — Imprimable gratuit (télécharger → `#contacto`)

---

## FAQ Accordion

6 questions, fonction `toggleFaq(el)` — ouvre une à la fois.  
Contenu trilingue via i18n.

---

## Produits actuels

| Nom | Prix | Image |
|-----|------|-------|
| Cartes émotions | USD $4.99 | `img/product-cartes.jpg` |
| Puzzle émotions | USD $6.99 | `img/product-emotions.jpg` |
| Mémoire animaux | USD $7.99 | `img/product-memory.jpg` |

> Plus de produits à venir — ne pas ajouter de faux produits sans confirmation.

---

## Déploiement

1. **GitHub** (privé): `git add -A && git commit -m "feat: ..." && git push origin master`
2. **Vercel**: Import automatique depuis GitHub. L'URL Vercel sera disponible après connexion à vercel.com.

---

## Tâches en attente / prochaines étapes

- [ ] Connecter Vercel au repo GitHub (l'utilisatrice doit faire l'import sur vercel.com)
- [ ] Cart + Stripe (Option B) — confirmé par l'utilisatrice, différé
- [ ] Plus de produits (quand disponibles)
- [ ] Témoignages (quand disponibles)
- [ ] Blog (futur)

---

## Notes importantes

- **Ne jamais modifier le style des mice** sans regenerer les images avec la palette exacte ci-dessus
- **`scaleX(-1)` sur l'`<img>` seulement** — pas sur le conteneur (#pR avec `right:0` + scaleX sur conteneur = bug de position)
- **Fichier unique** — tout est dans `index.html`, pas de build system, pas de npm
- **i18n** — toute nouvelle chaîne de texte doit avoir 3 traductions (fr/es/en) dans l'objet `T`
- **Liens legaux** (CGV, Confidentialité, Cookies) → `href="#"` intentionnel (pages futures)
