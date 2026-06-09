# Refonte thème Rack And Ride (base Dawn, Online Store 2.0)

Rack And Ride = supports muraux premium en chêne massif (surf/skate/snow/ski/vélo/wake-kite + déco),
marque des Landes (FR), marché européen (€). On **restyle/refactore Dawn**, on ne repart pas de zéro.
Synchro GitHub → Shopify.

## Plan en 6 phases
0. **Fondations** (FAIT)
1. **Chrome global** — header / annonce / footer / cart drawer
2. **Page produit** (CRO)
3. **Accueil**
4. **Collection & pages**
5. **Nettoyage + passe SEO**

## Décisions de design (appliquées en Phase 0)
- Palette « Chêne & Océan » : Blanc `#FFFFFF`, Blanc cassé `#F8F5F0`, Chêne `#B07C4C`, Encre `#1C2830`, Sauge `#7A8B6E`, Terracotta `#C16E4F` (badges promo uniquement)
- Direction premium : beaucoup de blanc, espace négatif généreux, accents chêne rares et précieux
- Polices self-hosted (RGPD) : **Fraunces** (titres) + **Work Sans** (corps) — pas de Google Fonts externe
- Arrondis doux (boutons ~6px, cartes ~12px) ; **PRIX EN ENCRE `#1F2B33`**, chêne réservé aux boutons/liens/focus
- Header en **« on scroll up »** (PAS « always ») ; panier = **cart drawer** ; largeur page **1400**
- Logo : utiliser le **vrai logo** (SVG inline depuis `/assets`), pas le nom en texte

## Multilingue — 4 langues actives : EN (défaut), FR, DE, ES
Toute chaîne visible → clé de traduction dans `locales/en.default.json` + `fr.json` + `de.json` + `es.json`.
**Jamais de texte en dur.**

## Exigences SEO (toutes phases)
- JSON-LD : Product + AggregateRating/Review, BreadcrumbList, Organization, FAQPage (collections), Article (blog)
- Collections : H1 = titre, bloc texte SEO intro + bloc texte riche pilotable en bas + bloc FAQ pilotable avec schema + breadcrumbs
- Jamais de title/meta en dur (`page_title`/`page_description` Shopify, remplissables par langue) ; ne pas casser le hreflang auto
- Perf/CWV : WebP responsive, lazy-load, `width`/`height` explicites, JS non-critique en `defer`
- 1 seul H1/page, ancres descriptives, `alt` depuis Shopify, avis avec balisage Review

## Metafields produit (Phase 2)
- `custom.dimensions` (type : single_line_text_field) → affiché en priorité dans l'accordéon Dimensions
  - Si vide → réglage de section `rar_dimensions_content` (richtext) → sinon clé `products.rar_accordions.dimensions_fallback`
  - À créer dans Shopify Admin > Settings > Custom data > Products

## Tags produits — filtres SEO
Les filtres de collection lisent les **tags des produits Shopify** via l'API Storefront Filtering.
Le thème intercepte `value.label` dans `snippets/facets.liquid` pour les filtres `filter.p.tag`
et fait un lookup dans `products.filter_tags.*` (locales storefront) via le handle du tag.

**Tags à appliquer sur chaque produit dans Shopify Admin** (texte exact, sensible à la casse) :

| Tag (EN — valeur stockée) | FR affiché | DE affiché | ES affiché |
|---|---|---|---|
| `Surfboard Wall Mounts` | Supports Muraux Surf | Surfboard-Wandhalterungen | Soportes de Pared para Surf |
| `Skateboard Wall Mounts` | Supports Muraux Skateboard | Skateboard-Wandhalterungen | Soportes de Pared para Skateboard |
| `Snowboard Wall Mounts` | Supports Muraux Snowboard | Snowboard-Wandhalterungen | Soportes de Pared para Snowboard |
| `Skis Wall Mounts` | Supports Muraux Skis | Ski-Wandhalterungen | Soportes de Pared para Esquís |
| `Bike Racks` | Racks Vélo | Fahrradständer | Soportes para Bicicleta |
| `Wake & Kite Racks` | Racks Wake & Kite | Wake & Kite Ständer | Soportes para Wake & Kite |
| `Home Deco` | Déco | Heimdekoration | Decoración |

⚠ Ne jamais changer la casse/orthographe des tags EN : le handle (`handleize`) doit correspondre
aux clés dans `locales/*.json` sous `products.filter_tags`. Ajouter un nouveau tag = ajouter
la clé dans les 4 fichiers de locale **et** configurer le filtre dans Shopify Admin → Search & Discovery.

## Rappels Phase 1
- Header **« on scroll up »**
- Barre d'annonce = « Enjoy 10% off on orders over €100 — use code SPRING10 » via clé de traduction, dans les 4 langues
