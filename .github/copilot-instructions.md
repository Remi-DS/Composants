# Instructions pour les agents IA — Canopée

## Règle absolue

Ne jamais inventer une règle, un composant, une variante, un token, un comportement ou une valeur du Design System.

## Avant toute conception

1. Identifier les composants nécessaires.
2. Consulter `ai/component-registry.json`.
3. Lire la documentation du composant avant de l'utiliser.
4. Respecter la hiérarchie des sources.
5. Signaler les contradictions et informations non confirmées.

## Réutilisation

- Réutiliser les composants Canopée existants.
- Ne pas créer une variante simplement parce que l'implémentation technique le permet.
- Une propriété d'API n'est pas une autorisation de design.
- Ne pas extrapoler une règle Prospect vers Client, ou Desktop vers Mobile, sans preuve.
- Ne pas inventer couleurs, typographies, espacements, rayons, dimensions ou breakpoints.

## Provenance

Toujours distinguer :
- règle de design documentée ;
- comportement d'implémentation ;
- observation ;
- recommandation ;
- information non confirmée.

En cas de contradiction, conserver la contradiction et demander une clarification plutôt que la résoudre arbitrairement.

## Prototypes

Pour un prototype utilisateur :
- privilégier la fidélité au Design System ;
- conserver les interactions nécessaires au test ;
- rendre les hypothèses explicites ;
- ne jamais présenter un comportement technique non vérifié comme une règle validée.

## Accessibilité

Vérifier notamment :
- structure sémantique ;
- navigation clavier ;
- ordre de tabulation ;
- états focus ;
- nom accessible ;
- contraste ;
- responsive ;
- lisibilité et tailles de cibles.

## Contrôle avant livraison

Vérifier composants, variantes, états, hiérarchie, contenu, dimensions, responsive, accessibilité, tokens et provenance.

Si un point n'est pas vérifiable, le dire explicitement.
