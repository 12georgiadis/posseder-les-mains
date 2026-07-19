[English](GUIDE_EN.md) · **Français** · [Manifeste](README.fr.md)

# Guide : monter une infrastructure de création IA pour une résidence ou un studio

*Blueprint générique, issu de deux dispositifs réels mis en place pour des résidences de création soutenues par le CNC. Les chiffres sont des ordres de grandeur datés (2026), à refiger avant toute décision. Compagnon du [Manifeste](README.fr.md).*

## 1. Les principes qui décident de tout

1. **Local d'abord** : les modèles ouverts sur une machine possédée couvrent l'essentiel de l'image, de la vidéo et du texte, gratuitement et sans filtre imposé.
2. **Location à l'heure, mutualisée** : pour le débordement, des GPU loués à l'heure dans des datacenters choisis. Une instance libérée sert à d'autres : rien ne tourne à vide. Jamais de serveur dédié au mois pour un usage ponctuel.
3. **Propriétaire en dose consciente** : les modèles fermés servent à comparer et à couvrir ce que l'ouvert ne fait pas encore, avec un plafond nominatif par personne.
4. **Tout se mesure ou se questionne** : voir la méthode carbone (§6).
5. **Le quotidien doit être trivial** : ouvrir le matin, tout retrouver. Si l'infrastructure se sent, elle est ratée.

## 2. L'architecture en quatre briques

**Brique 1, la machine possédée.** Une tour avec un GPU récent à large mémoire (en 2026 : NVIDIA RTX 5090 32 Go, 16 cœurs CPU, 96 Go de RAM, deux SSD NVMe). Elle porte la génération quotidienne, l'entraînement de styles personnels et les usages temps réel (peinture assistée, dont la latence interdit le distant). Système avec **image miroir** : configuré une fois, sauvegardé, restaurable à l'identique en minutes.

**Brique 2, les ordinateurs des participants (BYOD).** Mac Apple Silicon ou PC portable avec GPU NVIDIA, les deux conviennent. Ils portent les modèles de langage locaux (Ollama, LM Studio), les interfaces, et se branchent au reste par le réseau. Personne n'a besoin d'une machine puissante : le calcul lourd est ailleurs.

**Brique 3, le serveur loué (le « cloud maison »).** Un serveur GPU à l'heure chez un hébergeur transparent, avec un bureau graphique complet en streaming : fenêtres, explorateur de fichiers, navigateur. Linux ne veut pas dire ligne de commande ; le terminal n'existe que pour l'administrateur. Trois mécanismes le rendent vivable :
- la **golden image** : le système configuré une fois (outils, modèles, chemins), photographié, versionné (v1, v2, v3) ; on installe librement au quotidien, on fige après chaque évolution validée ; en cas de casse, on restaure la dernière bonne version ;
- le **volume persistant** : les données vivent hors du système ; éteindre la machine ne coûte que le stockage ; la rallumer prend deux minutes et tout est là ;
- la **séparation données/système** : une restauration ne touche jamais aux affaires de personne.

Géographie : le serveur interactif (bureau streamé) près des utilisateurs (depuis la France, un datacenter français : 10 à 15 ms) ; le calcul par lots (files de génération, entraînements) peut partir plus loin, là où l'électricité est la plus propre (hydraulique norvégien, géothermie islandaise, 25 à 65 ms indifférents pour du batch).

**Brique 4, le stockage commun.** Un NAS (36 To utiles en RAID), un dossier personnel par participant avec quota, monté partout, y compris dans le Finder ou l'Explorateur de chaque ordinateur (glisser-déposer, jamais de terminal). Contrainte physique à respecter : les modèles (5 à 30 Go pièce) vivent près du GPU qui les charge ; le NAS reçoit les créations, pas les checkpoints actifs.

**La règle d'or, enseignée le premier jour** : ce qui est à toi vit dans ton dossier, présent partout ; tout le reste peut être réinitialisé sans jamais te toucher.

## 3. Le réseau

- **Prérequis** : 1 Gbit/s symétrique minimum pour un groupe ; 8 Gbit/s symétrique recommandé (en France, disponible en offre pro mutualisée, parfois sans engagement ; la fibre dédiée 10 Gbit/s garantie impose des délais et des engagements incompatibles avec un usage court).
- **Un routeur à soi**, indépendant du lieu, avec un maillage privé chiffré (Tailscale) pour l'accès distant.
- Un switch multi-gigabit ; si le débordement est sur serveur loué, les téléchargements lourds se font côté datacenter et soulagent la fibre locale.

## 4. La pile logicielle (libre par défaut)

| Usage | Outils |
|---|---|
| Génération (moteur) | ComfyUI en mode serveur, interface dans le navigateur |
| Interface simple | SwarmUI (formulaire au-dessus du même ComfyUI, graphe complet pour les avancés) |
| Peindre avec l'IA | Krita + plugin krita-ai-diffusion (se branche au ComfyUI du serveur) |
| Monter | DaVinci Resolve (gratuit, Studio en option) ou la chaîne 100 % libre : Kdenlive, Audacity/Ardour, ffmpeg |
| Tout-en-un libre (labo) | Blender + Pallaidium (génération dans la timeline) |
| Langage local | Ollama, LM Studio, OpenWebUI |
| Agents | un harnais existant (Hermes Desktop et équivalents, clé à soi) ou le sien, construit avec Claude Code |
| Installation | Pinokio (un clic, et partage d'une app locale par lien ou QR code : un laptop devient une mini-plateforme pour le groupe) |

Vigilance : la licence du logiciel ne couvre pas celle des poids des modèles. Pour des œuvres destinées à une diffusion, tenir une liste de modèles aux poids réellement libres.

## 5. Les crédits : nominatifs, plafonnés, visibles

- **Un pool par usage** : génération (les crédits ComfyUI couvrent les appels de modèles distants depuis l'interface elle-même, avec le coût affiché sur chaque nœud, y compris en local), texte (OpenRouter, qui alimente aussi les harnais agentiques), orchestration (un abonnement agentique sérieux par personne, encadrants compris : ils doivent expérimenter au même niveau que les participants).
- **Quota par personne, alertes, plafond global.** Le coût visible par geste est un outil pédagogique, pas une punition.
- Vérifié en 2026 sur les modèles que j'ai testés : les agrégateurs répercutent le même tarif fournisseur au centime près. Le prix ne départage pas les plateformes ; la liberté (outils personnalisés, filtres), l'écosystème et la transparence carbone, si.

## 6. La méthode carbone : mesurer, calculer, estimer, questionner

| Niveau | Périmètre | Méthode |
|---|---|---|
| Mesuré | machines locales | relevé direct (`nvidia-smi`, CodeCarbon, wattmètre) |
| Calculé | serveurs loués transparents | heures × consommation × PUE publié × intensité carbone du lieu (sources réseau électrique national) |
| Estimé | API opaques | outils d'estimation (EcoLogits pour les LLM), proxy GPU-heure, hypothèses publiées, fourchettes |
| Questionné | les fournisseurs | demande formelle : localisation, mix électrique, PUE, CO2e par GPU-heure. Le silence est une donnée. |

Ordres de grandeur utiles : un mois de production locale intensive sur une station consomme environ 200 kWh, soit quelques kilos de CO2e sur un mix électrique très décarboné, et environ dix fois plus sur un mix moyen américain. Le lieu de l'électricité pèse plus que tout le reste. Et rappel du manifeste : bas-carbone ne veut pas dire propre.

## 7. Les prérequis (à exiger tôt)

**Côté participants** : un portable correct (Apple Silicon 16 Go ou PC RTX 8 Go de VRAM), 100 Go libres, le socle gratuit installé avant l'arrivée, aucune licence payante exigée.

**Côté lieu d'accueil** : les plans des espaces avant les arbitrages ; un circuit électrique pour la station (environ 1 kW en charge) ; le test d'éligibilité fibre à l'adresse exacte ; une pièce aérée (une station chauffe) qui ferme à clé ; l'accès en soirée et le week-end ; un vidéoprojecteur.

**Côté humain, le prérequis numéro un** : une personne dédiée et rémunérée pour monter, installer et maintenir (machine, images, serveur, réseau), distincte de l'accompagnement des usages. Sans ce poste, prendre les services clé en main et assumer leurs limites. Et décider **avant l'achat** qui hérite du matériel après.

## 8. Le budget, en blocs qui se coupent proprement

Ordres de grandeur 2026 pour un groupe d'une dizaine de personnes sur quatre à six semaines : la station (7 à 8 k€, dont la moitié pour le GPU et la RAM, tous deux volatils), l'équipement commun (écrans, NAS, réseau : 3 à 4 k€), les abonnements et crédits nominatifs (2,5 à 3,5 k€), la location GPU (0,3 à 0,9 k€ pour 100 à 300 heures), la connexion (négligeable en mutualisé), la mise en place et la maintenance (2 à 3 k€). Total réaliste : **15 à 22 k€** selon les curseurs. Les deux erreurs classiques : comprimer les crédits (le dispositif existe mais frustre) et oublier le poste humain (le dispositif meurt à la première panne).

## 9. Les leçons qui coûtent cher quand on les ignore

1. La RAM et les GPU sont volatils : verrouiller la fenêtre d'achat, garder une marge.
2. Tout installer et roder des mois avant, puis **refaire une passe de mise à jour à J-30** : en temps IA, six mois sont une éternité.
3. Ne jamais mettre les données dans l'image système.
4. Ne pas confondre bas prix de location au comptant (marketplaces interruptibles, carbone inconnu) et coût réel.
5. Pour un même modèle, les filtres de contenu suivent le fournisseur quel que soit l'agrégateur : si la liberté de création compte, elle se joue dans l'ouvert local, pas dans le choix de la plateforme.
6. Écrire aux fournisseurs. Les réponses (et les silences) changent les choix.

*Ismaël Joffroy Chandoutis. Texte sous licence CC BY-SA 4.0 : réutilisez, adaptez, citez.*
