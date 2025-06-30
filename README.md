# TP02 - Réveil matin

## 1 - Directives

### 1.1 - Déroulement du TP

- Remise du travail : lundi 7 juillet 2025, 23:59
- Ce travail est réalisé en équipe de 2 membres et seuls les membres de cette équipe y contribuent
- Toutes les réponses fournies doivent être originales (produites par l’étudiant ou un membre de l’équipe)
- Toute copie de code, de portion de code, d’algorithme ou de texte doit faire mention de sa source
- L’emprunt ou la copie de code ou de portions de code est interdite
- Tout constat de plagiat, tricherie ou fraude sera automatiquement déclaré à la Direction et les sanctions prévues seront appliquées
- Vous devez utiliser votre dépôt Git pour faire votre travail : si une situation particulière est détectée, vos commits moduleront votre note dans le groupe.
- Vous devez utiliser SharePoint pour gérer votre document de rapport (Onglet "Fichiers" de votre équipe Teams)
- La remise du travail doit être effectuée sur et à la date indiquée sur la plateforme d'enseignement Léa

### 1.2 - À remettre sur la plateforme d'enseignement Léa

- Un document Word contenant le détail du projet
- Votre code source C++ avec la structure de PlatformIO
- Vous devez fournir le lien d'une vidéo de 5 minutes illustrant le circuit, le code et le fonctionnement :
  - La vidéo doit être déposée sur YouTube
  - En lien non listé
  - La vidéo doit être accessible jusqu'à un an après la remise du travail
  - La vidéo doit être en français

### 1.3 - Structure de la remise

- **Vous devez remplir le fichier** `AUTHORS.md`. Il donne le nom et matricule des équipiers
- Votre code source doit être dans le répertoire `src` du présent dépôt Git
- Le répertoire source doit suivre la structure d’un projet PlatformIO
- Le lien de la vidéo doit être présent dans le document word et dans le fichier `AUTHORS.md` de votre dépôt Git
- La vidéo doit être déposée sur YouTube avec une option de partage « non listée »
- Le répertoire "documents" doit contenir votre rapport de TP

### 1.4 - Évaluation

L'évaluation du travail est effectuée par les enseignants de l'UE en se basant sur :

- L'historique de Git et de Teams/Sharepoint font office de référence pour évaluer la proportion du travail effectué par chaque équipier

- La qualité et le contenu du code source :

  - Conformité du code et des normes d'écriture utilisées dans le cours
  - Fonctionnalité du code
  - Facilité de lecture du code
  - Modularité
  - Modèle objet
  - Paramétrisation du code
  - Utilisation de constantes
  - Utilisation de fichiers de configuration
  - etc.

- La qualité et le contenu du document word :
  
  - Français
  - Schéma
  - Clarté et précision des explications
  - Mise en page
  - Page de présentation
  - etc.

- La qualité et le contenu de la présentation vidéo :

  - Vidéo
  - Audio
  - Explication orale
  - etc.

- Participation au code : la participation est évaluée en utilisant GitInspector. Chaque étudiant doit être au dessus de 45% de participation ou verra sa note ajustée. Les codes faits en classe lors des exercices sont enlevés des statistiques pour ne pas pénaliser le partenaire qui ne met pas son ancien code. (Ex. Bouton.\[h|cpp], DEL.\[h|cpp])

Tout partage de code, d'explications, de bouts de texte, etc. est considéré comme du plagiat. Pour plus de détails, consultez le site (et ses vidéos) [Sois intègre du Cégep de Sainte-Foy](http://csfoy.ca/soisintegre) ainsi que [l'article 6.1.12 de la PÉA](https://www.csfoy.ca/fileadmin/documents/notre_cegep/politiques_et_reglements/5.9_PolitiqueEvaluationApprentissages_2019.pdf)

Si vous utilisez un code donné en cours, n'oubliez pas de référencer son origine dans votre code.

## 2 - Description du projet

Ce projet est un réveil matin qui permet de programmer des alarmes à des heures
précises. Il se présente sous la forme d'un boîtier avec un afficheur 4 digits,
3 boutons et 2 DELs. Il propose aussi une interface série et une interface web
qui permettent de contrôler les alarmes.

Au déclenchement de l'alarme, une musique est démarrée. Les boutons permettent
de stopper la musique et de relancer l'alarme (Snooze) après un délai de 5
minutes, puis 3 minutes, puis au 1 minute (On veut vraiment le réveiller !). 

Pour arrêter définitivement l'alarme, il faut appuyer longuement sur le troisième
bouton pendant au moins 5 secondes.

### 2.1 - Démarrage du réveil

Au démarrage, le réveil se connecte sur le réseau WiFi et récupère l'heure
actuelle avec une requête NTP.

La configuration du WiFi se fait dans le fichier `config.h` qui n'est pas
versionné (donc dans le fichier .gitignore). Vous devez fournir un fichier
`config.h.template` qui contient les informations de connexion au réseau WiFi
ainsi que les instructions de création du fichier `config.h`.

Durant la connexion, l'afficheur 4 digits affiche une animation de 3 images qui
cyclent :

- les 4 segments d de chaque digit s'allument seuls
- les 4 segments g de chaque digit s'allument seuls
- les 4 segments a de chaque digit s'allument seuls

Si la connexion WiFi échoue ou dépasse 30 secondes, le réveil affiche 88:88
clignotant sur l'afficheur 4 digits à la fréquence de 1 Hz.

### 2.2 - Fonctionnement

Une fois démarré, le réveil affiche l'heure actuelle sur l'afficheur 4 digits de
l'ESP32. À l'appui du bouton 1, le réveil affiche la première alarme disponible.
Pour indiquer que l'affichage du 4 digits est en mode "Affichage d'alarmes", une
DEL verte clignote. Si l'alarme est activée, une DEL rouge est allumée.

À l'appui du bouton 1, le réveil affiche la prochaine alarme disponible. Une
fois arrivé à la dernière alarme, le réveil revient à l'affichage de la première
alarme.

À l'appui du bouton 2, le réveil active/désactive l'alarme affichée. Le nouveau
statut de l'alarme est indiqué par la DEL rouge. Plusieurs alarmes peuvent être
activées en même temps. (Plusieurs heures de réveil pour être sûr de se lever !
Heures de prises de médicaments, etc.)

À l'appui du bouton 3, le réveil sort du mode "Affichage d'alarmes" et revient à
l'affichage de l'heure actuelle.

Les alarmes sont enregistrées dans un fichier JSON de la mémoire flash de
l'ESP32.

### 2.3 - Atteinte de l'heure d'une alarme

Lorsque l'heure d'une alarme est atteinte, quelque soit l'action en cours, une
musique se déclenche et l'heure s'affiche. Les boutons permettent de stopper la
musique et de relancer l'alarme (Rappel/Snooze) après un délai de 5 minutes,
puis 3 minutes, puis toute les minutes (On veut vraiment le réveiller !). Pour
arrêter définitivement l'alarme, il faut appuyer longuement sur le troisième
bouton pendant au moins 5 secondes. L'alarme est alors considérée comme étant
"désactivée" (Elle doit être réactivée pour la journée suivante).

Deux DELS sont nécessaires. Durant la période de sélection de l'alarme, un DEL
verte clignote. Une DEL rouge est allumée lorsqu'une alarme est activée.

Lorsque l'heure de l'alarme est arrivée, une musique de l'alarme débute. Les
trois boutons peuvent arrêter la musique temporairement. L'alarme est relancée
(Snooze) après un délai de 5 minutes, puis 3 minutes, puis au 1 minute (On veut
vraiment le réveiller !). Pour arrêter définitivement l'alarme, il faut appuyer
longuement sur le troisième bouton pendant au moins 5 secondes.

### 2.4 - Interfaces de contrôle

#### 2.4.1 - Interface série

Le réveil doit aussi être accessible par le port série. Vous devez minimalement
proposer les commandes suivantes (Vous avez le droit d'utiliser la classe
"BasicCommandInterpretor du dépôt
https://github.com/PiFou86/420-W48-SF-Utilitaires-Demo/) :

- `alarme get` : affiche les alarmes par ordre croissant d'heure
- `alarme set hh:mm` : ajoute/active une alarme
- `alarme unset hh:mm` : désactive une alarme
- `alarme delete hh:mm` : supprime une alarme
- `alarme update hh:mm hh:mm` : modifie une alarme (ancienne heure, nouvelle heure)
- `help` : affiche les commandes disponibles
- `time` : affiche l'heure actuelle
- `network` : affiche les informations de connexion WiFi
- `reboot` : redémarre le réveil

#### 2.4.2 - Interface web

Un contrôle à distance propose une page web qui :

- Affiche les alarmes par ordre croissant d'heure
- Permet d'activer / désactiver une alarme
- Permet d'ajouter/modifier des alarmes
- Permet de supprimer des alarmes

Remarque vous devez empêcher deux alarmes à la même heure.

Le site web doit utiliser les technologies et architectures suivantes :

- HTML
- CSS
- JavaScript
- AJAX
- REST

Voici un exemple de l'interface web :

![Interface web](img/exemple_affichage.png)

### 2.5 - Bonus

- Une musique différente par alarme (Jusqu'à 5%)
- À votre imagination mais avec de la valeur ajoutée (Jusqu'à 5%)

## 3 - Répartition des points

1. Document word (18%)

- Contexte du projet (2%)
- Planification, attribution des tâches (3%)
- Description des étapes du projet (3%)
- Diagramme de classes (5%)
- Schéma circuit et schéma du montage (5%)

2. Registre des heures consacrées au projet (2%)

Le registre doit indiquer la répartition des tâches. Le registre doit montrer les tâches respectives que chaque personne aura fait avec le nombre d’heures par tâche.

Il doit correspondre aux données collectées dans l'historique de Git et de Teams/Sharepoint de l'équipe.

3. Vidéo de 5 minutes illustrant le fonctionnement (10%)

- Présentation rapide du circuit (3%)
- Présentation rapide de la structure du code et des choix de conception (4%)
- Présentation du fonctionnement (3%)

4. Code (70%)

- Démarrage du programme - connexion WiFi (2%)
- Démarrage du programme - récupération de l'heure (3%)
- Affichage de l'heure actuelle (5%)
- Affichage des alarmes - Plaquette (5%)
- Activation/désactivation des alarmes - Plaquette (5%)
- Affichage des alarmes - Interface série (5%)
- Activation/désactivation des alarmes - Interface série (5%)
- Affichage des alarmes - Interface web (5%)
- Activation/désactivation des alarmes - Interface web (5%)
- Suppression des alarmes - Interface web (5%)
- Modification des alarmes - Interface web (5%)
- Alarme fonctionnelle (5%)
- Rappel/Snooze (5%)
- Arrêt définitif de l'alarme (5%)
- Sauvegarde des alarmes (5%)

5. Bonus: mise en oeuvre de choix de musiques différentes par l'alarme (Jusqu'à 5%)

6. Bonus: mise en oeuvre d'une fonction supplémentaire originale (Jusqu'à 5%)
