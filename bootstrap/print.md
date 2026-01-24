```
Voici les deux approches principales :

1. L'approche "Tout Bootstrap" (Sans CSS personnalisé)
Si votre page est simple, vous pouvez ajouter la classe .d-print-none à tous les éléments que vous voulez ignorer (nav, footer, sidebars) et laisser votre section cible telle quelle.

HTML
<nav class="navbar d-print-none">...</nav>

<section id="section-a-imprimer">
  <h1>Mon contenu important</h1>
  <p>Ce texte sera sur le papier.</p>
</section>

<footer class="d-print-none">...</footer>



2. L'approche CSS (Plus rapide pour les pages complexes)
Si vous avez beaucoup d'éléments, il est plus simple d'inverser la logique en CSS. On cache tout ce qui se trouve dans le body lors de l'impression, puis on force l'affichage de la section voulue.
Pour imprimer une seule section spécifique avec Bootstrap, la méthode la plus propre consiste à masquer tout le contenu par défaut pour l'impression, sauf l'élément que vous souhaitez conserver.

Ajoutez ce code dans votre fichier CSS :

CSS
@media print {
  /* 1. On cache absolument tout le contenu du body */
  body * {
    visibility: hidden;
  }

  /* 2. On réaffiche uniquement la section cible et ses enfants */
  #section-a-imprimer, #section-a-imprimer * {
    visibility: visible;
  }

  /* 3. On repositionne la section en haut à gauche de la page */
  #section-a-imprimer {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
  }
}
3. Ajouter un bouton d'impression "propre"
Pour offrir une bonne expérience utilisateur, vous pouvez ajouter un bouton (qui se cache lui-même lors de l'impression) pour lancer la commande :

HTML
<button onclick="window.print()" class="btn btn-primary d-print-none">
  Imprimer cette section
</button>
Conseils bonus pour un rendu parfait :
Supprimer les marges forcées : Parfois, les navigateurs ajoutent des en-têtes (date, titre). Vous pouvez les réduire en ajoutant @page { margin: 0; } dans votre bloc @media print.

Couleurs : Bootstrap désactive souvent les couleurs de fond à l'impression. Utilisez la classe utilitaire print-color-adjust: exact; en CSS si vous voulez forcer l'apparition des couleurs.

```
