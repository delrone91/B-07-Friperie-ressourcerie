# Carnet de bord J1 · Appareillage


Un carnet par binôme, rempli au fil de l'eau avec vos propres mots. Une phrase honnête (« j'ai essayé X, j'ai vu Y, je ne comprends pas pourquoi ») vaut mieux qu'une phrase parfaite recopiée. Aucune donnée personnelle, aucune clé ni jeton, ni l'adresse complète que `dsh web` affiche (elle contient un jeton). C'est aussi votre journal de décisions (astuce 13) : ce que vous avez demandé, ce qui a cassé, ce que vous avez refusé, et pourquoi.

Binôme : b07

Thème provisoire et public visé : friperies et ressourceries. L'assistant sert aux clients des friperies et ressourceries qui veulent connaître les prix, les horaires, les styles proposés et les lieux.

Trois questions auxquelles l'assistant pourrait répondre :
1. Quels sont les prix habituels en friperie ?
2. Quels sont les horaires d'ouverture ?
3. Où trouver une friperie vintage ?

Rôles de départ et moments d'échange : Delrone manipule, Pierre-Yves vérifie. On échange les rôles environ toutes les 20 minutes (au premier échange, l'autre arrête le serveur avec Ctrl+C et le relance avec `npm start`).

## Cahier personnel (remis par le formateur en J1-01)

Recopiez les valeurs telles que le formateur vous les a remises. Ne les changez pas, ne les échangez pas avec un autre binôme.

- Limite de caractères d'un message (le nombre N) : 200
- Premier mot reconnu, en plus de « salut », « aide » et « test » : mission
- Second mot reconnu : chemin

## Commandes essayées

Notez le dossier de lancement, la commande et sa sortie exacte, surtout quand un outil a bloqué.

- Dossier : `atelier`
- Commande et résultat : `node --version` → `v24.21.0` (24.20 minimum demandé).
- Dossier : `atelier`
- Commande et résultat : `npm start` lance le serveur et la page de départ s'affiche dans le navigateur.
- Dossier : `atelier`
- Commande et résultat : `npm test` lance les 9 tests du serveur et les 9 passent sans échec.
- Dossier : racine du paquet
- Commande et résultat : `git checkout 3347ada -- atelier/public` remet les fichiers de la page dans leur état de départ (voir la décision de J1-01).

Pour chaque checkpoint : cochez la case quand toute la preuve de la fiche est réunie, collez la preuve (texte, commande ou phrase), puis notez ce que vous avez prédit, essayé, observé, et une difficulté qui reste.

## Le chat web (N0 Subir)

### J1-01 · 🧭 Équipage — [fiche](checkpoints/J1-01-equipage.md)

- [x] Validé
- Preuve (page de départ affichée sur votre poste, cahier personnel recopié ci-dessus) : La page Cap Web s'affiche dans le navigateur avec le statut « Votre point de départ est prêt. » et le cahier personnel b07 est recopié plus haut.
- Le `p#status` est-il vide dans le HTML ? Qui écrit sa phrase ? Oui, dans `index.html` la balise `<p id="status" role="status"></p>` est vide. C'est le script `public/js/app.js` qui écrit la phrase « Votre point de départ est prêt. » quand la page se charge.
- Décision prise ensemble : La page de l'atelier avait été modifiée trop tôt (titre, liste, couleurs, commit « étape 3 ») alors que J1-01 demande de ne pas la toucher et que les modifications de J1-03 se font dans `essais-n0`. Nous avons remis `atelier/public` dans son état de départ.
- Difficulté qui reste : pas de difficulté technique pour lancer la page (l'échange de rôles avec Ctrl+C puis `npm start` a marché). La difficulté a été l'organisation : la page de l'atelier a été modifiée trop tôt puis remise à zéro.

### J1-02 · 💬 Premier prompt — [fiche](checkpoints/J1-02-premier-prompt.md)

- [x] Validé
- Preuve : `essais-n0/chatbot-v1.html` s'ouvre ; le prompt et la première réponse sont collés ci-dessous, en entier, sans donnée personnelle.
- Mon prompt, tel quel : « Fais-moi un chatbot pour les clients des friperies et ressourceries, dans une seule page HTML que j'ouvre dans mon navigateur. »
- La première réponse du chat (texte et code), telle quelle : Sur Le Chat de Mistral, il n'a écrit aucun texte avant le code. Il a répondu directement avec un canvas nommé « Chatbot Friperie & Ressourcerie » qui contient ce code.

```html
<!doctype html>
<html lang="fr">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Frip'Bot — Assistant Friperie & Ressourcerie</title>
<style>
  :root {
    --vert: #2e7d5b;
    --vert-clair: #e6f4ee;
    --ocre: #d98324;
    --texte: #22302a;
    --fond: #f4f7f4;
    --bulle-bot: #ffffff;
    --bulle-user: #2e7d5b;
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    font-family: "Segoe UI", system-ui, -apple-system, sans-serif;
    background: var(--fond);
    color: var(--texte);
    display: flex; justify-content: center;
    min-height: 100vh; padding: 16px;
  }
  .app {
    width: 100%; max-width: 640px;
    display: flex; flex-direction: column;
    height: 90vh; min-height: 540px;
    background: #fff; border-radius: 18px;
    box-shadow: 0 8px 30px rgba(30,60,40,.12);
    overflow: hidden;
  }
  header {
    background: var(--vert); color: #fff;
    padding: 16px 20px; display: flex; align-items: center; gap: 12px;
  }
  header .logo {
    width: 42px; height: 42px; border-radius: 50%;
    background: #fff; color: var(--vert);
    display: flex; align-items: center; justify-content: center;
    font-size: 22px; flex-shrink: 0;
  }
  header h1 { font-size: 1.05rem; }
  header p { font-size: .78rem; opacity: .85; }
  header .statut { margin-left: auto; text-align: right; font-size: .72rem; opacity: .9; }
  header .point { display: inline-block; width: 8px; height: 8px; border-radius: 50%; background: #9fe3c0; margin-right: 4px; }

  #chat {
    flex: 1; overflow-y: auto; padding: 18px;
    background: var(--fond);
    display: flex; flex-direction: column; gap: 10px;
  }
  .msg {
    max-width: 82%; padding: 11px 14px;
    border-radius: 16px; font-size: .92rem; line-height: 1.45;
    white-space: pre-line; word-wrap: break-word;
    animation: apparition .25s ease;
  }
  @keyframes apparition { from { opacity: 0; transform: translateY(6px);} to { opacity: 1; transform: none;} }
  .bot { background: var(--bulle-bot); align-self: flex-start; border-bottom-left-radius: 4px; box-shadow: 0 2px 6px rgba(0,0,0,.06); }
  .user { background: var(--bulle-user); color: #fff; align-self: flex-end; border-bottom-right-radius: 4px; }
  .bot b { color: var(--vert); }
  .suggestions { display: flex; flex-wrap: wrap; gap: 8px; margin-top: 4px; }
  .suggestions button {
    border: 1.5px solid var(--vert); background: var(--vert-clair);
    color: var(--vert); font-size: .78rem; padding: 6px 12px;
    border-radius: 999px; cursor: pointer; transition: .15s;
  }
  .suggestions button:hover { background: var(--vert); color: #fff; }

  .saisie {
    display: flex; gap: 8px; padding: 12px;
    background: #fff; border-top: 1px solid #e3e9e5;
  }
  .saisie input {
    flex: 1; padding: 12px 16px; border-radius: 999px;
    border: 1.5px solid #cfdcd4; font-size: .92rem; outline: none;
  }
  .saisie input:focus { border-color: var(--vert); }
  .saisie button {
    background: var(--vert); color: #fff; border: none;
    width: 46px; height: 46px; border-radius: 50%;
    font-size: 1.1rem; cursor: pointer; transition: .15s;
  }
  .saisie button:hover { background: #1f5c41; }
  footer { text-align: center; font-size: .68rem; padding: 6px; color: #8a9b91; background: #fff; }
</style>
</head>
<body>
<div class="app">
  <header>
    <div class="logo">♻️</div>
    <div>
      <h1>Frip'Bot — La Friperie Solidaire</h1>
      <p>Ton assistant pour acheter, donner et trier en seconde main</p>
    </div>
    <div class="statut"><span class="point"></span>En ligne<br>réponse en quelques secondes</div>
  </header>

  <div id="chat"></div>

  <div class="saisie">
    <input id="entree" type="text" placeholder="Pose ta question (horaires, dons, prix...)" autocomplete="off" />
    <button id="envoyer" title="Envoyer">➤</button>
  </div>
  <footer>Assistant informatif — les détails peuvent varier selon chaque boutique.</footer>
</div>

<script>
const chat = document.getElementById("chat");
const entree = document.getElementById("entree");
const envoyer = document.getElementById("envoyer");

const BOUTIQUE = {
  nom: "La Friperie Solidaire",
  horaires: "mardi–vendredi 10h–18h30, samedi 10h–17h, fermé dimanche et lundi",
  adresse: "12 rue des Recyclés",
  tel: "01 23 45 67 89",
  email: "bonjour@friperie-solidaire.fr",
};

const REGLES = [
  {
    mots: ["bonjour", "salut", "coucou", "hello", "bonsoir", "hey"],
    reponse: () => `Bonjour et bienvenue chez ${BOUTIQUE.nom} ! 👋\nJe peux t'aider sur :\n• Les horaires et l'adresse\n• Les dons (vêtements, livres, objets)\n• Les prix et le dépôt-vente\n• Le tri et la réparation\n• Les avantages de la seconde main\nSur quoi puis-je te renseigner ?`,
    suggestions: ["Quels sont vos horaires ?", "Comment donner des vêtements ?", "Comment ça marche le dépôt-vente ?"]
  },
  {
    mots: ["horaire", "heure", "ouvert", "ferme", "ouvre", "dimanche", "lundi", "samedi"],
    reponse: () => `🕒 Nous sommes ouverts :\n<b>${BOUTIQUE.horaires}</b>.\nTu nous trouves au ${BOUTIQUE.adresse}. Passe nous voir !`,
    suggestions: ["Où êtes-vous situés ?", "Puis-je déposer un don maintenant ?"]
  },
  {
    mots: ["adresse", "où", "situation", "situer", "localisation", "trouver", "plan", "acces"],
    reponse: () => `📍 Nous sommes au <b>${BOUTIQUE.adresse}</b>, facilement accessible à pied et en transports.\nPour nous joindre : ${BOUTIQUE.tel} ou ${BOUTIQUE.email}.`,
    suggestions: ["Quels sont vos horaires ?", "Y a-t-il un parking à proximité ?"]
  },
  {
    mots: ["don", "donner", "dons", "deposer don", "ceder", "vêtement a donner", "vetement a donner", "débarras", "debaras"],
    reponse: () => `🎁 Pour donner, c'est simple :\n1. Trie tes affaires (vêtements, linge, chaussures, livres, vaisselle, petits objets)\n2. Vérifie qu'ils sont <b>propres et en bon état</b>\n3. Dépose-les en boutique pendant les horaires d'ouverture, ou dans la borne de collecte devant le magasin\n⚠️ Nous n'acceptons pas : matelas, produits chimiques, objets cassés ou très abîmés.\nMerci pour ton geste solidaire ! ♻️`,
    suggestions: ["Que n'acceptez-vous pas ?", "Puis-je avoir un reçu fiscal ?"]
  },
  {
    mots: ["recu", "fiscal", "deduction", "impot", "defiscalisation"],
    reponse: () => `🧾 Oui ! Pour les dons en bon état, nous pouvons te remettre un reçu fiscal (formulaire Cerfa) qui te permet de bénéficier d'une réduction d'impôt de <b>66 % du montant estimé</b>, dans la limite de 61 % de ton revenu imposable. Demande-le à l'accueil lors du dépôt.`
  },
  {
    mots: ["prix", "combien", "coute", "tarif", "cher", "pas cher", "budget"],
    reponse: () => `💰 Nos prix sont très accessibles, car nous valorisons les dons :\n• T-shirts / pulls : 2 à 5 €\n• Pantalons / jeans : 4 à 8 €\n• Vestes / manteaux : 6 à 15 €\n• Chaussures : 3 à 10 €\n• Livres : 1 à 3 €\nLes prix varient selon l'état et la marque. Une étiquette couleur indique le rayon et la fourchette de prix. Des promotions régulières sont affichées en boutique !`,
    suggestions: ["Comment marche le dépôt-vente ?", "Faites-vous des soldes ?"]
  },
  {
    mots: ["depot", "depot-vente", "depot vente", "consignation", "vendre mes", "vendre"],
    reponse: () => `🤝 Le dépôt-vente en 3 étapes :\n1. Apporte tes articles (propres, en bon état) pour évaluation\n2. Nous fixons un prix ensemble ; l'article reste en vente 6 semaines\n3. S'il se vend, tu touches <b>50 % du prix de vente</b> (chèque ou crédit boutique à +10 %)\nLes invendus sont soit repris, soit donnés à l'association.\nTu peux rapporter tes articles du mardi au vendredi après-midi.`,
    suggestions: ["Quels articles acceptez-vous ?", "Combien de temps dure la mise en vente ?"]
  },
  {
    mots: ["etat", "qualite", "tache", "trou", "abime", "use", "dechirure", "manque bouton"],
    reponse: () => `🔍 Ce que nous acceptons : articles <b>propres, complets et portables</b> (petits défauts réparables OK, ex. bouton manquant).\nCe que nous refusons : articles déchirés, tachés durablement, mouillés ou sentant le renfermé.\n💡 Un article avec un petit défaut peut partir au <b>rayon "à réparer"</b> ou en atelier couture — demandes-nous !`,
    suggestions: ["Proposez-vous des ateliers réparation ?"]
  },
  {
    mots: ["reparation", "reparer", "atelier", "couture", "retouche", "troc", "repair cafe"],
    reponse: () => `🪡 Nous proposons :\n• Un <b>atelier couture</b> gratuit le samedi matin (10h–12h) — viens avec ton article à réparer\n• Des retouches simples payantes à petit prix\n• Des tutos réparation sur l'ardoise à l'entrée du magasin\nProlonger la vie d'un vêtement, c'est le plus écologique des gestes !`
  },
  {
    mots: ["ecologique", "ecologie", "environnement", "seconde main", "durable", "impact", "pourquoi friperie", "utile"],
    reponse: () => `🌍 Acheter seconde main, ça change tout :\n• Fabriquer un t-shirt neuf = ~2 700 L d'eau ; d'occasion = quasi zéro\n• Prolonger la vie d'un vêtement de 9 mois réduit de 20–30 % son empreinte carbone\n• En France, ~10 kg de textiles par personne finissent à la poubelle chaque année — la plupart pourrait être réemployés\n• Ton achat soutient aussi l'emploi local et l'insertion.\nMerci de faire partie de la solution ! ♻️`,
    suggestions: ["Comment donner des vêtements ?", "Comment marche le dépôt-vente ?"]
  },
  {
    mots: ["taille", "essayer", "cabine", "essayage", "pointure"],
    reponse: () => `👗 Oui, nous avons des <b>cabines d'essayage</b> ! Les tailles varient selon les époques et les marques (un 38 des années 80 ≠ un 38 actuel), donc essaie toujours quand c'est possible. En cas de doute, le matin les rayons sont plus fournis et l'équipe peut t'aider à dénicher la bonne taille.`
  },
  {
    mots: ["paiement", "carte", "especes", "cheque", "payer", "virement"],
    reponse: () => `💳 Nous acceptons : espèces, carte bancaire (à partir de 5 €) et chèques. Certains dons de carte (bons cadeaux solidaires) sont aussi utilisables en caisse.`
  },
  {
    mots: ["rendre", "retour", "remboursement", "echange"],
    reponse: () => `🔄 Articles d'occasion vendus en l'état : ni repris ni échangés, sauf défaut caché non signalé — dans ce cas présente-toi avec le ticket sous 7 jours et nous trouvons une solution (échange ou avoir boutique).`
  },
  {
    mots: ["benevolat", "benevole", "travailler", "jobs", "emploi", "stage"],
    reponse: () => `❤️ Rejoins l'équipe ! Nous cherchons régulièrement des bénévoles pour : tri, mise en rayon, accueil, atelier couture. Aucune expérience requise, 2 h/semaine suffisent. Passe nous voir ou écris à ${BOUTIQUE.email} !`
  },
  {
    mots: ["tri", "jeter", "poubelle", "recycler", "dechets", "textile usager"],
    reponse: () => `🗑️ Le bon réflexe tri :\n• <b>Portable</b> → donne en friperie / ressourcerie\n• <b>Abîmé mais propre</b> → borne de collecte textile (recyclage en isolant, chiffons...)\n• <b>Mouillé / sale / cassé</b> → déchetterie ou ordures selon la matière\nJamais de textile dans la poubelle grise s'il existe une borne à proximité !`
  },
  {
    mots: ["meuble", "electro", "electromenager", "appareil", "hi-fi", "livre", "jouet", "vaisselle"],
    reponse: () => `🛋️ Nous acceptons aussi : petits meubles transportables à la main, petits appareils électriques <b>testés et fonctionnels</b>, livres, jeux et jouets complets, vaisselle sans éclat.\nPour les gros électroménagers ou meubles volumineux, renseigne-toi en boutique : une collecte peut être organisée une fois par mois.`
  },
  {
    mots: ["merci", "super", "parfait", "genial", "top", "cool"],
    reponse: () => `Avec plaisir ! 😊 Si tu as d'autres questions, je suis là. Et n'hésite pas à venir discuter avec l'équipe en boutique — le café est souvent offert aux donateurs !`
  },
  {
    mots: ["au revoir", "bye", "a plus", "ciao", "a bientot"],
    reponse: () => `À bientôt chez ${BOUTIQUE.nom} ! 👋 Merci de penser seconde main. ♻️`
  },
];

function normaliser(t) {
  return t.toLowerCase()
    .replace(/[éèê]/g, "e").replace(/[àâ]/g, "a").replace(/î/g, "i")
    .replace(/[ô]/g, "o").replace(/û/g, "u").replace(/ç/g, "c")
    .replace(/['’]/g, " ").replace(/\s+/g, " ").trim();
}

function trouverReponse(texte) {
  const t = normaliser(texte);
  if (!t) return null;
  let meilleur = null, score = 0;
  for (const regle of REGLES) {
    for (const mot of regle.mots) {
      if (t.includes(normaliser(mot)) && mot.length > score) {
        score = mot.length; meilleur = regle;
      }
    }
  }
  if (meilleur) return { texte: meilleur.reponse(), suggestions: meilleur.suggestions || null };
  return {
    texte: `Hmm, je n'ai pas encore la réponse à cette question 😅\nEssaie avec des mots comme « horaires », « don », « prix », « dépôt-vente », « tri » ou « réparation » — ou contacte-nous au ${BOUTIQUE.tel} / ${BOUTIQUE.email}.`,
    suggestions: ["Quels sont vos horaires ?", "Comment donner des vêtements ?", "Quels sont vos prix ?"]
  };
}

function ajouterMessage(contenu, role, suggestions) {
  const div = document.createElement("div");
  div.className = "msg " + role;
  if (role === "bot") {
    div.innerHTML = contenu;
    if (suggestions) {
      const s = document.createElement("div");
      s.className = "suggestions";
      suggestions.forEach(txt => {
        const b = document.createElement("button");
        b.textContent = txt;
        b.onclick = () => { s.remove(); envoyerTexte(txt); };
        s.appendChild(b);
      });
      div.appendChild(s);
    }
  } else {
    div.textContent = contenu;
  }
  chat.appendChild(div);
  chat.scrollTop = chat.scrollHeight;
}

function envoyerTexte(texte) {
  if (!texte.trim()) return;
  ajouterMessage(texte, "user");
  entree.value = "";
  setTimeout(() => {
    const r = trouverReponse(texte);
    ajouterMessage(r.texte, "bot", r.suggestions);
  }, 350);
}

envoyer.onclick = () => envoyerTexte(entree.value);
entree.addEventListener("keydown", e => { if (e.key === "Enter") envoyerTexte(entree.value); });

ajouterMessage(
  `Bonjour ! 👋 Je suis <b>Frip'Bot</b>, l'assistant de ${BOUTIQUE.nom}.\nJe réponds à tes questions sur les horaires, les dons, les prix, le dépôt-vente, le tri ou la réparation.\nQue veux-tu savoir ?`,
  "bot",
  ["Quels sont vos horaires ?", "Comment donner des vêtements ?", "Comment marche le dépôt-vente ?", "Pourquoi acheter en seconde main ?"]
);
</script>
</body>
</html>
```

- Trois lignes d'observation (ce que j'ai vu en utilisant la page) :
  1. J'ai été surpris d'avoir une réponse cohérente quand j'ai parlé du prix, alors que la page ne fait aucun appel à une API. Tout est écrit d'avance dans la page.
  2. Quand j'ai écrit « vous me les prenez pour combien », il m'a répondu avec la liste des prix de vente en boutique.
  3. Dès qu'on pousse un peu, on trouve très vite la limite. Quand j'ai demandé « peut import la marque du vêtement ? », il a répondu qu'il n'avait pas encore la réponse et m'a donné un numéro de téléphone et une adresse mail.
  4. Message hors thème (ajouté par Pierre-Yves en vérifiant) : à « Quelle est la capitale du Japon ? », il répond « Hmm, je n'ai pas encore la réponse à cette question 😅 » et propose des mots-clés (« horaires », « don », « prix », « dépôt-vente », « tri », « réparation ») ainsi qu'un téléphone et un e-mail inventés. Il ne sort pas de son thème, mais il ne dit pas non plus que la question est hors sujet.
- Difficulté qui reste : sous WSL, le double-clic sur `chatbot-v1.html` dans VS Code ouvre le code et pas la page ; il a fallu coller le chemin `\\wsl.localhost\...` dans le navigateur.

### J1-03 · 💥 Ça marche… jusqu'à quand — [fiche](checkpoints/J1-03-jusqua-quand.md)

- [x] Validé
- Liste de contrôle de la version 1 (cinq à huit comportements essayés) :
  1. Au chargement, un message d'accueil s'affiche avec 4 boutons de suggestion. ✔
  2. La touche Entrée et le bouton ➤ envoient le message. ✔
  3. Mon message apparaît dans le chat et le champ se vide. ✔
  4. « horaires », « don » et « reçu fiscal » reçoivent la bonne réponse. ✔
  5. « prix » ou une question avec « combien » donne la liste des prix. ✔
  6. Un clic sur un bouton de suggestion envoie sa question et le bot y répond. ✔
  7. Un message vide n'est pas envoyé. ✔
  8. Une question inconnue donne « je n'ai pas encore la réponse », avec un téléphone et une adresse mail. ✔
- Journal des régressions, une entrée par modification : ce que j'ai demandé · ce qui marche maintenant · ce qui marchait et ne marche plus · ce que je n'avais pas vu, et comment je l'ai trouvé.
  - Modification 1 (`chatbot-v2.html`). Pierre-Yves travaille seul sur J1-03 (il manipule et vérifie) ; la conversation Mistral de J1-02 n'étant pas sur son poste, il a ouvert une nouvelle conversation et y a collé le code de `chatbot-v1.html`. Mistral a d'abord répondu « votre message ne précise pas ce que vous souhaitez que j'en fasse » et proposé 4 choix ; aucun n'a été pris.
    - Ce que j'ai demandé : « Ajoute un bouton Effacer qui vide la conversation. » Mistral a répondu « C'est fait ✅ » avec le code complet, et a conseillé de « le copier pour remplacer votre fichier » : nous l'avons mis dans un nouveau fichier, comme le demande la fiche.
    - Ce qui marche maintenant : un bouton « 🧹 Effacer » dans le bandeau vert ; au clic, la conversation est vidée et le message d'accueil revient avec ses 4 boutons. Les 8 lignes de la liste de contrôle sont toujours OK.
    - Ce qui marchait et ne marche plus : la mise en page. Le bouton a été ajouté dans le bandeau vert, qui est devenu plus haut et a débordé sur la zone de conversation. En rouvrant la v2 et la v3 côte à côte plus tard, le bandeau s'affichait correctement dans les deux (même CSS) : le débordement dépend de la taille de la fenêtre, il n'apparaît pas à chaque fois.
    - Ce que je n'avais pas vu, et comment je l'ai trouvé : je n'avais demandé qu'un bouton, pas de changement de mise en page ; je l'ai vu en regardant la page entière après le test, pas en testant le bouton lui-même.
  - Modification 2 (`chatbot-v3.html`), même conversation Mistral.
    - Ce que j'ai demandé : « Garde les messages quand je recharge la page. » Mistral a répondu « C'est fait ✅ La conversation est maintenant sauvegardée dans le navigateur (localStorage) et restaurée au rechargement de la page », en affirmant que les boutons de suggestion « restent cliquables » et qu'Effacer vide aussi la sauvegarde.
    - Ce qui marche maintenant : après 2 messages puis F5, les messages sont toujours là. Effacer puis F5 donne un seul message d'accueil (pas de doublon). Les 8 lignes de la liste de contrôle et le bouton Effacer de la v2 sont toujours OK.
    - Ce qui marchait et ne marche plus : (1) la conversation n'est gardée qu'une fois : 2 messages, F5, un 3e message, F5 → tout a disparu sauf le dernier message. (2) Un bouton de suggestion sur lequel on a cliqué disparaît, mais il réapparaît après F5 sur le message d'avant.
    - Ce que je n'avais pas vu, et comment je l'ai trouvé : le premier F5 marchait, donc la nouveauté avait l'air de fonctionner ; le problème n'apparaît qu'en envoyant un message après un rechargement, puis en rechargeant encore. Je l'ai trouvé en enchaînant F5 → message → F5, pas en testant la nouveauté une seule fois.
  - Modification 3 (`chatbot-v4.html`), même conversation Mistral. Limite de notre cahier personnel : 200.
    - Ce que j'ai demandé : « Refuse les messages de plus de 200 caractères. » Mistral a répondu « C'est fait ✅ Les messages de plus de 200 caractères sont maintenant refusés », avec une constante `MAX_CARACTERES`, un texte d'aide dans le champ (« 200 caractères max »), et a proposé une autre solution (`maxlength="200"`) à laquelle nous n'avons pas répondu.
    - Ce qui marche maintenant : un message trop long n'apparaît pas dans le chat ; le bot répond un avertissement avec le nombre de caractères (par exemple « (202) »), et le texte reste dans le champ pour être raccourci. Après F5, l'avertissement est toujours là.
    - Ce qui marchait et ne marche plus : une question courte suivie de beaucoup d'espaces est refusée : « horaires » + des espaces donne l'avertissement « (299) » alors que la vraie question fait 8 lettres (avant, elle recevait les horaires). Le bug de la v3 n'est pas corrigé : le bouton « Puis-je avoir un reçu fiscal ? » réapparaît après F5 sur le message précédent.
    - Ce que je n'avais pas vu, et comment je l'ai trouvé : la limite compte aussi les espaces, alors que le message vide, lui, est détecté en enlevant les espaces ; je l'ai trouvé en essayant exprès un message court rempli d'espaces. Un texte que je croyais faire 201 caractères en faisait 202 : j'ai dû générer des textes de longueur exacte pour tester la limite.
- Chasse à l'angle mort (ce qui a été trouvé, et par qui) : Pierre-Yves, seul (pas de voisin disponible), sur `chatbot-v4.html` :
  - message vide : pas envoyé ✔ ;
  - message trop long (202 caractères) : refusé avec un avertissement, le texte reste dans le champ ;
  - « horaires » suivi de beaucoup d'espaces : refusé (« 299 »), alors que la question est courte ✘ ;
  - rechargements (F5) enchaînés avec des messages : les anciens messages disparaissent, les boutons de suggestion déjà cliqués réapparaissent ✘ ;
  - en relisant le code avec un assistant IA, une piste non reproduite à la main : si on clique sur Effacer pendant que le bot « réfléchit » (0,35 s), sa réponse arrive quand même dans la conversation vidée. Trop rapide pour le tester à la main : noté comme non vérifié.
  - Pas essayé : `<b>gras</b>`, deux messages très rapides, fenêtre à 360 px.
- Deux phrases de conclusion : la modification 2 (garder les messages au rechargement) a cassé le plus de choses, alors que Mistral affirmait « C'est fait ✅ » et que le premier F5 semblait marcher. Sans la liste de contrôle et sans enchaîner F5 → message → F5, je ne l'aurais pas su : il aurait fallu qu'un utilisateur perde sa conversation pour s'en rendre compte.
- Difficulté qui reste : la conversation Mistral de J1-02 n'était pas sur mon poste, j'ai dû repartir d'une nouvelle conversation ; tester une limite de 200 caractères demande des textes de longueur exacte (un texte « de 201 » en faisait 202) ; le débordement du bandeau vert n'apparaît pas à chaque fois, ce qui le rend difficile à prouver.

### J1-04 · 🎲 Même prompt, autre réponse — [fiche](checkpoints/J1-04-meme-prompt.md)

- [x] Validé
- Le prompt de référence (identique aux trois essais) : « Fais-moi un chatbot pour les clients des friperies et ressourceries, dans une seule page HTML que j'ouvre dans mon navigateur. » — recopié mot pour mot depuis J1-02, collé tel quel dans trois nouvelles conversations Mistral.
- Le tableau des écarts (trois colonnes A, B, C ; au moins quatre critères ; des faits, pas des impressions) : trois nouvelles conversations Mistral, codes collés tels quels dans `essais-n0/essai-A.html`, `essai-B.html`, `essai-C.html`. Mêmes essais sur les trois pages : « horaires », « Quelle est la capitale du Japon ? », message vide, F5, largeur 360 px (F12 puis Ctrl+Shift+M).

  | Critère | A | B | C |
  |---|---|---|---|
  | Structure du code | 304 lignes, un fichier, script en bas, 17 règles de mots-clés | 363 lignes, un fichier, script en bas, infos boutique regroupées en haut du script, 16 intentions (Mistral en annonce 15) | 298 lignes, un fichier, script en bas, 11 règles + 3 règles de politesse |
  | Réponse à « horaires » | horaires : mardi–vendredi 10h–18h30, samedi 10h–17h, fermé dimanche et lundi | horaires : mardi–vendredi 10h–18h30, samedi 9h30–19h, fermé dimanche et lundi | horaires : **lundi**–vendredi 10h–18h30, samedi 10h–17h, fermé dimanche |
  | Hors thème (« capitale du Japon ») | « Je n'ai pas bien compris votre question 😅 » + liste de sujets | « Hmm, je ne suis pas sûr d'avoir compris 😅 » + exemples de questions | 3 réponses différentes pour 3 fois la même question (phrase tirée au hasard dans le code) |
  | Message vide | pas envoyé | pas envoyé | pas envoyé |
  | F5 (rechargement) | retour au message d'accueil, conversation perdue (pas de `localStorage`) | idem | idem |
  | Largeur 360 px | zone des messages petite, il faut défiler ; le bandeau du haut cache une partie des messages et des suggestions | idem A | le bandeau du haut et la zone de saisie ne prennent pas toute la largeur ; quelques problèmes de marges et de débordement |
  | Ce qui manque | pas de bouton Effacer, pas de mémoire, pas de limite de caractères | idem | idem, et pas d'animation « en train d'écrire » |
  | Ce qui diffère (noms, textes, ton) | bouton « Envoyer » ; adresse « 12 rue de la Récup', 75000 Ville » ; réponses du bot affichées avec `innerHTML` | bouton « ➤ » ; boutique à Lyon, téléphone en 04 ; réponses affichées avec `textContent` | bouton « Envoyer » ; « Ressour**s**erie » mal écrit 5 fois (dont le titre) ; pas d'adresse ; heure affichée sur chaque message ; Mistral tutoie dans sa réponse (A et B : vouvoiement) |
- Une phrase de conclusion (ce que ces écarts autorisent, ce qu'ils interdisent de supposer) : avec le même prompt, on peut compter sur un chatbot à mots-clés qui répond aux horaires et ignore le message vide, mais on ne peut rien supposer du reste (les horaires eux-mêmes, l'orthographe, le nombre de règles, l'affichage sur téléphone, ni même que la description de Mistral corresponde à son code) : chaque version doit être vérifiée.
- Difficulté qui reste : je ne savais pas comment tester une largeur de 360 px ; il a fallu découvrir le mode « appareil » des outils du navigateur (F12 puis Ctrl+Shift+M). Les trois pages ont été testées par Pierre-Yves seul.

## L'agent (N1 Demander)

### J1-05 · 🛠 dsh en main — [fiche](checkpoints/J1-05-dsh-en-main.md)

- [ ] Validé
- Preuve (`dsh --version`, mode Read Only, modèle `capweb-ia`, `git status -- atelier` propre ; **jamais la clé**) :
- La consigne exacte envoyée à l'agent et sa réponse :
- Pour chaque fichier cité : existe ou non, description juste ou fausse, pourquoi ; et un fichier qu'il n'a pas cité :
- Difficulté qui reste :

### J1-06 · 🧱 Anatomie d'un prompt — [fiche](checkpoints/J1-06-anatomie-dun-prompt.md)

- [ ] Validé
- Preuve (deux prompts, deux résultats, grille remplie, commit du squelette) :
- Prompt vague et ce que montre la page (trois lignes, fichiers touchés) :
- Prompt structuré, en six parties, tel qu'envoyé :
- Les hypothèses de l'agent, et ma réponse :
- La grille (✔ ou ✘ et un mot, pour « vague » puis « structuré ») :
- Une phrase : entre les deux résultats, ce qui a le plus changé, c'est… parce que la partie… de mon prompt disait…
- Difficulté qui reste :

### J1-07 · 👣 Petits pas — [fiche](checkpoints/J1-07-petits-pas.md)

- [ ] Validé
- Preuve (découpage écrit avant la première demande, trois diffs relus, un refus écrit, un commit par étape acceptée, trois boutons de questions qui fonctionnent) :
- La tâche, mes trois questions et mon découpage en trois étapes (écrit avant la première demande d'écriture) :
- Ce que l'agent a proposé comme découpage, ce que j'ai gardé, pourquoi :
- Mon refus écrit : ce que l'agent avait fait, pourquoi je le refuse, ce que j'ai demandé à la place :
- Difficulté qui reste :

**Journal des décisions.** Une ligne par demande faite à l'agent, de J1-07 à J1-09 (les trois étapes de J1-07, puis la correction de J1-08, puis les six demandes de J1-09) : la demande copiée, le diff relu (fichiers, nombre de lignes, une chose que je n'avais pas demandée ?), le verdict et pourquoi.

| N° | Demande | Diff relu | Verdict et pourquoi |
|---|---|---|---|
| 1 | | | |
| 2 | | | |
| 3 | | | |
| 4 | | | |
| 5 | | | |
| 6 | | | |
| 7 | | | |
| 8 | | | |
| 9 | | | |
| 10 | | | |

### J1-08 · 🔎 Revue de la page — [fiche](checkpoints/J1-08-revue-de-la-page.md)

- [ ] Validé
- Preuve (trois défauts, un corrigé avec son avant et son après, diff relu, revue adverse vérifiée) :
- Mes défauts, un par ligne :

  | Lentille (structure, clavier, écrans) | Où (élément ou fichier) | Comment je l'ai vu |
  |---|---|---|
  | | | |
  | | | |
  | | | |

- La revue adverse : trois affirmations de l'agent, la référence qu'il a donnée (fichier, ligne), mon verdict (vrai, faux, rejeté sans référence) et comment j'ai vérifié :
- Le défaut corrigé : l'avant (capture ou valeur), ma demande ciblée (copiée), le diff relu (fichiers, lignes, changement non demandé ?), l'après (même geste, même mesure) :
- Difficulté qui reste :

### J1-09 · 🧠 Un cerveau à règles, par prompts — [fiche](checkpoints/J1-09-cerveau-a-regles.md)

- [ ] Validé
- Preuve (comportements vérifiés : « Vous : … », message vide, `<b>gras</b>`, mes deux mots, ma limite ; `/js/brain.js` et `/js/view.js` affichés ; F5 ; « Effacer ») :
- Mes six demandes et leurs verdicts : dans le journal des décisions ci-dessus.
- Le rôle de chaque fichier, en une phrase chacun :
  - `app.js` :
  - `brain.js` :
  - `view.js` :
- Ce que j'ai vu quand j'ai mis `{pas du json` dans la mémoire :
- Difficulté qui reste :

### J1-10 · 🧪 Épreuve de l'explication — [fiche](checkpoints/J1-10-epreuve-explication.md)

- [ ] Validé
- Preuve (`npm test` vert avec cinq tests dont ma limite, commit de sauvegarde, remise faite) :
- Le test rouge : son nom, son message exact, et ce qu'il m'a appris :
- Épreuve de l'explication, éditeur fermé :
  - Ce que je n'ai pas su expliquer :
  - Ce que mon binôme n'a pas su expliquer :
- Difficulté qui reste :

## Quatre questions pour finir

1. Pourquoi `textContent` et pas `innerHTML` ?
2. Pourquoi trois fichiers plutôt qu'un seul ?
3. L'agent a écrit le code : comment savez-vous qu'il est juste, et qu'est-ce qui l'a vu échouer ?
4. Quelle astuce avez-vous le plus utilisée aujourd'hui, et laquelle avez-vous oubliée ?

## Aides utilisées

- Indices, aide-mémoire, voisins :
- Ce que j'ai demandé à une IA, et comment j'ai vérifié sa réponse :

## Notes personnelles (chacun)

Pour préparer l'explication de votre part du code. Chacun écrit avec ses mots.

- Nom :
- Ce que j'ai compris :
- Ce que je n'ai pas encore compris :

- Nom :
- Ce que j'ai compris :
- Ce que je n'ai pas encore compris :

Git sert à sauvegarder chaque étape acceptée : lisez les différences et nommez les fichiers à enregistrer, jamais `git add -A`. Attendez la consigne du formateur avant tout envoi vers un dépôt commun.

[README du jour](README.md) · [Aide-mémoire HTML/CSS](ressources/aide-memoire.md) · [Aide-mémoire JavaScript](ressources/aide-memoire-js.md) · [Notice dsh](ressources/dsh.md)
