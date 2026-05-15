# Adéquat Taxi — Site optimisé

## Résumé des améliorations

| Métrique | Avant | Après | Gain |
|---|---|---|---|
| Poids du HTML | ~1 000 KB | 91 KB | **-91 %** |
| Poids total page (HTML + images) | ~1 300 KB | ~220 KB | **-83 %** |
| Images base64 dans le HTML | 7 inlines | 0 (toutes externalisées) | Cache navigateur ✓ |
| Cache statique Netlify | Inexistant | 1 an immutable | Réduit charge serveur |
| Formulaire de réservation | Aucun envoi 🚫 | Netlify Forms + email + fallback WhatsApp ✅ | Tu reçois les demandes |

## ⚠️ Important : le HTML utilisait encore les anciennes images SNI Taxi

Tes vraies images (Adéquat Taxi avec le logo "AT TAXI") étaient bien dans `assets/`, mais le HTML continuait d'afficher les **photos en base64 de l'autre site SNI Taxi**. C'est corrigé : le hero, les cartes véhicules et le footer utilisent maintenant tes vraies images Adéquat.

## 1. Images

Toutes converties en WebP optimisé (~3-4× plus léger que le JPG original) :

| Fichier | Format | Poids | Usage |
|---|---|---|---|
| `adequat-logo.webp` | WebP | 8 KB | Logo header (redimensionné 600×600) |
| `adequat-logo.png` | PNG | 70 KB | Apple touch icon (fallback) |
| `adequat-logo-white.webp` | WebP transparent | 16 KB | Logo footer (texte blanc + or préservé) |
| `eco-taxi-lumineux.webp` | WebP | 42 KB | **Hero** (Tesla avec lumière TAXI sur le toit) |
| `eco-taxi-lumineux.jpg` | JPEG | 87 KB | Fallback og:image (réseaux sociaux) |
| `eco-electrique.webp` | WebP | 39 KB | Carte véhicule Tesla |
| `business-van.webp` | WebP | 45 KB | Carte véhicule Mercedes |
| `favicon.webp` | WebP | 3 KB | Onglet navigateur |

**Choix éditorial** : j'ai mis `eco-taxi-lumineux.jpg` (la Tesla avec la lumière "TAXI Clermont-Ferrand" sur le toit) en photo principale du hero. C'est ta meilleure photo pour la conversion : elle dit immédiatement "vrai taxi pro" au visiteur dès l'arrivée sur le site.

## 2. Logo footer corrigé

Avant, le CSS appliquait `filter:invert(1) brightness(2)` pour rendre le logo visible sur le fond noir du footer. Mais ce filtre transformait aussi la touche **dorée du "T"** en bleu/violet — affichage cassé.

J'ai créé une vraie version "dark mode" du logo (`adequat-logo-white.webp`) :
- Fond blanc → transparent
- Lettre "A" noire → blanche
- **"T" doré préservé** comme dans l'original

## 3. SEO complet pour Adéquat Taxi

Ajouté dans le `<head>` :
- **JSON-LD `TaxiService`** complet : nom, téléphone (+33 6 98 35 29 90), email (adequat.taxi@outlook.fr), SIRET (877918243), zones desservies, horaires 24/7, paiement CPAM accepté → permet l'affichage enrichi sur Google.
- **Geo tags** Clermont-Ferrand (FR-63, coordonnées 45.7772/3.0870).
- **Open Graph** + **Twitter Card** : quand on partage le lien sur WhatsApp/FB/LinkedIn, l'aperçu affiche la photo Tesla + titre + description.
- **Canonical URL** + meta robots.
- **Mots-clés ciblés** : "Adéquat Taxi", "taxi conventionné CPAM", "Aulnat", "VSL", "dialyse", "La Bourboule", "Mont-Dore", "Vichy".
- **Favicon** depuis ton fichier favicon.png.
- **`<link rel="preload">`** sur la photo hero → améliore le score Core Web Vitals (LCP).

## 4. Performance (Core Web Vitals)

- `loading="lazy"` sur images sous la ligne de flottaison
- `decoding="async"` partout
- `fetchpriority="high"` sur la photo hero
- `width` + `height` sur **toutes les images** → empêche le layout shift (CLS)
- `preconnect` ajouté pour `fonts.gstatic.com`

## 5. Sécurité & cache (`netlify.toml`)

Avant, le netlify.toml faisait juste `publish = "."` (rien d'autre).

Après :
- Cache HTML : revalidation à chaque visite (toujours frais)
- Cache assets/ : **1 an immutable** (très efficace, économise du trafic)
- Headers de sécurité : `X-Content-Type-Options`, `Referrer-Policy`, `Permissions-Policy`, `Strict-Transport-Security`, `X-Frame-Options`
- Minification CSS/JS/HTML automatique activée

## 6. Formulaire de réservation : maintenant fonctionnel ✅

Avant, le bouton "Envoyer ma demande" affichait juste un message visuel mais **n'envoyait rien nulle part**. Aucune demande client n'arrivait chez toi.

Après : **Netlify Forms** intégré (gratuit, 100 demandes/mois). Chaque demande t'arrive automatiquement par email à `adequat.taxi@outlook.fr`.

### Étapes d'activation (UNE SEULE FOIS, ~3 min)

1. **Déploie** ce nouveau ZIP sur Netlify (drag-drop ou push Git).
2. Au prochain déploiement, Netlify détecte automatiquement le formulaire grâce à `data-netlify="true"`.
3. Dashboard Netlify → **Forms** → tu dois voir un formulaire `reservation` apparaître.
4. Clique dessus → **Settings & usage** → **Add notification** → **Email notification** → mets `adequat.taxi@outlook.fr` → enregistre.

À partir de là :
- ✅ Chaque demande t'arrive **par email** (vérifie tes spams le 1er coup)
- ✅ Toutes les soumissions sont **archivées** dans le dashboard Netlify (CSV téléchargeable)
- 🤖 **Anti-spam honeypot** déjà en place (champ caché bloquant les bots)
- 📱 **Fallback WhatsApp** : si l'envoi échoue, le client a une popup "Envoyer par WhatsApp ?" avec sa demande pré-remplie vers ton 06 98 35 29 90 → tu reçois la demande dans tous les cas.

### Test
Remplis ton propre formulaire après le redéploiement. Tu dois voir :
1. Toast "✅ Demande envoyée !"
2. Un email arrive à adequat.taxi@outlook.fr
3. La soumission apparaît dans Forms → reservation

## ⚠️ À FAIRE manuellement

### 1. Domaine canonique
J'ai utilisé `https://www.adequat-taxi.fr/` comme placeholder. Si ton domaine est différent (ou si tu utilises encore le sous-domaine `*.netlify.app`), fais un rechercher/remplacer dans `index.html` sur les **6 occurrences** de `www.adequat-taxi.fr` :
- balise `<link rel="canonical">`
- meta `og:url`, `og:image`, `twitter:image`
- JSON-LD : `image`, `logo`, `url`

### 2. Activer Netlify Forms
Voir section 6 ci-dessus. Sans cette étape, tes demandes n'arrivent pas dans ta boîte mail.

### 3. Google My Business
Pour ressortir sur Google Maps et les recherches "taxi Clermont-Ferrand" → crée/optimise une fiche **Google My Business**. C'est le levier #1 pour les taxis en local.

### 4. Si tu as Facebook / Instagram
Ajoute les URLs dans le tableau `sameAs` du JSON-LD :
```json
"sameAs": [
  "https://www.facebook.com/adequattaxi",
  "https://www.instagram.com/adequattaxi"
]
```

### 5. SIRET partagé avec SNI Taxi ?
J'ai remarqué que le SIRET (877 918 243) est **identique** entre tes deux sites SNI et Adéquat Taxi. Si c'est volontaire (deux marques pour la même entreprise), tout va bien. Si c'est une erreur de copier-coller, pense à corriger dans `index.html` (footer) et `JSON-LD` (`identifier`).
