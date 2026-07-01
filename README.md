# Top 8 — Rumeurs mercato NBA

Widget interactif **drag & drop** : glisse 8 rumeurs de transfert NBA pour composer ton
classement (de la plus crédible à la plus folle). Souris **et** tactile.

## Données & photos

- **Données joueurs** : API [balldontlie.io](https://www.balldontlie.io/) (identité, poste, équipe).
- **Photos** : l'API balldontlie **ne fournit pas** de portraits. Les headshots officiels
  proviennent du **CDN NBA** (`cdn.nba.com/headshots/...`) et sont **hébergés dans le repo**
  (`img/`) pour rendre le widget autonome et permettre la génération de l'image partageable
  (le CDN NBA n'expose pas de CORS, ce qui « tainte » sinon le canvas).

## Fonctionnalités

- Drag & drop unifié souris / tactile (sans dépendance)
- Partage **X / Facebook / WhatsApp**, copie du texte
- **Téléchargement d'une image PNG** du classement (canvas) + **partage natif** mobile (Web Share API)
- Largeur max 620 px, thème clair, accent rouge NBA, responsive mobile (2 colonnes)
- Auto-hauteur en iframe (`postMessage` → `{type:'top8-height'}`)

## Intégration WordPress (bloc « HTML personnalisé »)

```html
<iframe
  id="top8-nba"
  src="https://jcrochet-netizen.github.io/top8-nba-mercato/"
  title="Top 8 des rumeurs mercato NBA"
  loading="lazy"
  scrolling="no"
  referrerpolicy="strict-origin-when-cross-origin"
  style="width:100%;max-width:640px;display:block;margin:0 auto;border:0;overflow:hidden;min-height:900px;"></iframe>

<script>
(function () {
  var ORIGIN = 'https://jcrochet-netizen.github.io';
  var frame  = document.getElementById('top8-nba');
  window.addEventListener('message', function (e) {
    if (e.origin !== ORIGIN) return;
    var d = e.data;
    if (!d || d.type !== 'top8-height') return;
    var h = parseInt(d.height, 10);
    if (!h || h < 1) return;
    if (!frame) frame = document.getElementById('top8-nba');
    if (frame) frame.style.height = h + 'px';
  }, false);
})();
</script>
```

## SEO

Le contenu d'une iframe n'est pas attribué à la page hôte par Google : encadrer l'iframe
d'un `H2` + intro + liste HTML crawlable des 8 rumeurs. Voir les bonnes pratiques d'embed
(origine vérifiée, `loading=lazy`, `min-height`, `scrolling=no`, centrage).

## Personnalisation

Tout est en haut du `<script>` de `index.html` : tableau `ITEMS` (joueur, rumeur, image),
`CONFIG.shareUrl` (URL de la page hôte pour les partages), `CONFIG.hashtags`.
