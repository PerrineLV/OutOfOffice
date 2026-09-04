# Charte graphique — Out of Office

Référence pratique pour coder l'interface. Les jetons CSS ci-dessous vivent aussi comme fichier prêt à importer dans `docs/design-tokens.css`.

Source complète avec les planches visuelles (couverture, système de logo, palette, typographie, interface, règles d'usage) : [page Notion](https://app.notion.com/p/3d1d51b4ae0381b2b946e6f969df55b3). Fichier source éditable : [Canva](https://canva.link/45671u7npiqk938).

## Positionnement

- Nom de marque : Out of Office. Logotype : `out of office`. Forme courte : OOO.
- Signature principale (FR) : « La rencontre, sans les collègues. »
- Promesse (EN) : Meet people outside your workplace.
- Personnalité : discrète, complice, moderne, rassurante, légèrement insolente.
- Idée centrale : le statut OOO signale une disponibilité choisie, hors du bureau.

## Système de logo

Trois expressions complémentaires : le wordmark `out of office` (formats où le nom doit être immédiatement compris), le monogramme OOO (icône d'app, favicon, avatar, petits formats), le badge de statut OOO (signale qu'une personne est disponible dans l'app).

Règles : espace libre suffisant autour du logo, monogramme OOO toujours identifiable, ne pas étirer, éviter ombres/contours/effets chargés et décorations parasites, vérifier le contraste en clair comme en sombre.

## Couleurs

| Jeton | Couleur | Valeur | Usage recommandé |
| --- | --- | --- | --- |
| `--color-cream` | Warm cream | `#FFF7F0` | Fond du mode clair, texte sur fond sombre |
| `--color-ink` | Ink navy | `#0E1B2A` | Fond du mode sombre, texte principal, éléments structurants |
| `--color-coral` | Coral | `#FF6B6B` | Actions, likes, accents (texte Ink navy dessus) |
| `--color-lavender` | Lavender | `#A78BFA` | Focus, actions secondaires, accents sur fond Ink navy |
| `--color-mint` | Mint | `#B8F2E6` | Confirmation, état activé, accents sur fond Ink navy |

Mode clair : fond cream, texte ink, accents coral/lavender/mint. Mode sombre : fond ink, texte cream, mêmes accents.

**Règle d'usage la plus importante (accessibilité) :** Coral, Lavender et Mint ne sont pas assez contrastés pour du texte sur fond cream (ratios entre 1,17:1 et 2,62:1, décoration uniquement dans ce sens). Sur fond Ink navy en revanche, les trois passent très largement le seuil de 4,5:1 et conviennent au texte courant. Toujours utiliser Ink navy pour du texte posé directement sur une surface Coral/Lavender/Mint.

Autres règles accessibles : viser 4,5:1 texte courant / 3:1 grand texte, ne jamais coder un état avec la couleur seule (toujours un libellé, éventuellement une icône), focus clavier visible d'au moins 2px, cibles tactiles d'au moins 44×44px CSS, respecter `prefers-reduced-motion`.

## Typographie

| Style | Police | Taille / interligne | Usage |
| --- | --- | --- | --- |
| H1 | Space Grotesk Bold | 40 / 44 px | Titre principal |
| H2 | Space Grotesk Bold | 28 / 32 px | Titre de section |
| H3 | Space Grotesk Bold | 20 / 24 px | Sous-section, carte |
| Body | DM Sans Regular | 16 / 24 px | Texte courant, formulaires |
| Small | DM Sans Regular | 14 / 20 px | Métadonnées, aide |
| Caption | DM Sans Regular | 12 / 16 px | Information secondaire non essentielle |

Interlignage du corps : 150 %. Tracking display : environ `-0.01em` (à valider visuellement). Tracking small : environ `0.01em`.

**Exception accessible :** ne jamais utiliser le style Caption (12px) pour un label de formulaire, un message d'erreur ou une action, même secondaire. minimum 14px dans ces cas.

## Interface

Grille d'espacement : 8, 16, 24 px (`--space-1/2/3`). Deux thèmes complets (clair/sombre), le changement suit le système par défaut et reste modifiable manuellement. Navigation principale : Découverte, Likes, Messages, Profil. Cartes de profil lisibles avec métadonnées séparées de la bio. Tags de profil (exemples) : Apéros, Live music, Sport, Voyages, Café, Lecture.

Comportements accessibles à respecter dès l'implémentation : chaque icône de navigation a un libellé accessible, l'onglet actif est indiqué par texte + style + `aria-current` (jamais la couleur seule), les badges de statut ont un texte lisible par les technologies d'assistance, les labels de formulaire restent visibles même après saisie, les erreurs sont décrites près du champ et annoncées aux lecteurs d'écran.

## Ton éditorial et microcopie

Ton direct, léger, rassurant ; peut détourner le vocabulaire du bureau sans minimiser confidentialité, consentement ou sécurité.

- Confidentialité : « Votre vie privée d'abord. Nous n'affichons jamais vos collègues. »
- Exclusion : « Éviter mes collègues »
- Disponibilité : « Visible pour les autres quand vous êtes disponible. »
- Statut : « OOO active »

## Points non tranchés, à trancher avec Perrine avant de coder l'écran concerné

La charte source signale elle-même ces points comme non harmonisés. Ne pas trancher à sa place, demander :

- Deux signatures FR concurrentes existent dans les planches (« La rencontre, sans les collègues. » recommandée comme principale, « Dating, out of office. » à réserver aux contenus courts/bilingues) : confirmer avant d'écrire la home ou le App Store listing.
- Ce que « OOO active » signifie précisément pour la visibilité d'un profil n'est pas défini.
- Le comportement du badge OOO en mode discret et hors ligne n'est pas documenté.
- Les planches Canva affichent des dates de version incohérentes (avril 2024 et mai 2024) et quelques coquilles à corriger avant d'intégrer un texte d'exemple tel quel dans le produit.