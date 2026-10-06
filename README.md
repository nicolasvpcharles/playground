Physics Simulator
Un petit jeu / sandbox 2D développé en C++ avec raylib.
Le projet mélange plusieurs systèmes : physique simple, gravité, collisions, caméra, ennemis, vagues, tir, particules, score et pièces.
> Le projet est encore en développement. Certaines mécaniques sont volontairement simples et certaines fonctionnalités sont encore en cours d'amélioration.
🎮 Aperçu
Le joueur contrôle un personnage dans une arène 2D.
Il peut :
se déplacer grâce à la physique ;
viser avec la souris ;
tirer avec le clic gauche ;
être repoussé par le tir ;
subir des dégâts en touchant certains ennemis ;
récupérer des pièces ;
gagner du score ;
interagir avec des murs et le sol.
Le jeu utilise une caméra qui suit le joueur.
🛠️ Technologies
C++
raylib
`std::vector`
`std::chrono`
`std::cmath`
📦 Prérequis
Un compilateur C++ compatible
raylib 6.0 ou une version compatible
VS Code ou un autre IDE
Sous Windows, un environnement MSYS2 / MinGW peut être utilisé.
Le projet a été développé sous Windows avec MinGW.
🚀 Compilation
Avec MinGW et raylib correctement installés :
```bash
g++ main.cpp -o main.exe -IC:/msys64/ucrt64/include -LC:/msys64/ucrt64/lib -lraylib -lopengl32 -lgdi32 -lwinmm
```
Les chemins peuvent être différents selon l'installation de raylib.
🕹️ Contrôles
Action	Contrôle
Viser	Souris
Tirer	Clic gauche
Plein écran	F11
La position de la souris est convertie en coordonnées du monde grâce à `GetScreenToWorld2D()`, ce qui permet de viser correctement lorsque la caméra bouge.
⚙️ Fonctionnement général
Le programme suit une boucle de jeu classique :
```text
Initialisation
      ↓
Création des objets
      ↓
Boucle principale
      ↓
Gestion des entrées
      ↓
Calcul du delta time
      ↓
Mise à jour de la physique
      ↓
Détection des collisions
      ↓
Mise à jour des ennemis
      ↓
Mise à jour des particules
      ↓
Dessin
      ↓
Image suivante
```
Le programme utilise `GetFrameTime()` pour récupérer le temps écoulé entre deux images.
🧑‍🚀 Joueur
Le joueur est représenté par la classe `player`.
Elle contient notamment :
sa position ;
sa taille ;
sa vitesse horizontale et verticale ;
la gravité ;
sa vie ;
ses munitions ;
son coefficient de restitution ;
sa couleur.
La gravité est appliquée avec :
```cpp
velocityY += gravity * dt;
```
Puis la position est mise à jour avec :
```cpp
x += velocityX * dt;
y += velocityY * dt;
```
💥 Recul
Lorsque le joueur tire, une force est appliquée dans la direction du tir.
Le programme calcule une direction normalisée afin que la force garde une intensité constante quelle que soit la distance entre le joueur et la souris.
🔫 Arme et tir
L'arme du joueur est orientée vers la souris.
Le programme utilise `atan2()` pour calculer l'angle :
```cpp
atan2(dy, dx)
```
L'arme est ensuite dessinée avec `DrawRectanglePro()`.
Le clic gauche déclenche le tir si le joueur possède des munitions.
Le tir consomme une munition et crée également des particules orange.
Les munitions sont rechargées automatiquement avec le temps jusqu'à `maxBulets`.
✨ Particules
Les particules sont gérées par la classe `Particle`.
Chaque particule possède :
une position ;
une vitesse ;
une durée de vie ;
un rayon ;
une couleur.
La durée de vie diminue avec `dt`.
Lorsqu'elle atteint `0`, la particule est supprimée.
Son opacité diminue également progressivement pour créer un effet de disparition.
🧱 Murs
Les murs sont représentés par la classe `wall`.
Chaque mur possède :
une position ;
une largeur ;
une hauteur ;
une couleur ;
un état de collision.
Le joueur utilise `checkWallCollision()` pour détecter les collisions avec les murs.
La position précédente du joueur est utilisée pour déterminer si la collision vient du dessus, du dessous, de la gauche ou de la droite.
👾 Ennemis
Le projet possède plusieurs types d'ennemis.
Ennemi normal
La classe `enemyA` représente un ennemi basique qui se déplace horizontalement vers la gauche.
Une collision avec le joueur inflige des dégâts.
🌀 Spinning Enemy
La classe `slidingEnemy` représente un ennemi qui se déplace horizontalement tout en tournant.
Il possède notamment :
une position ;
une taille ;
une vitesse ;
une rotation ;
une couleur.
Les collisions utilisent une détection de type SAT (Separating Axis Theorem) via `playerVsSlidingEnemy()`.
Une collision avec cet ennemi inflige des dégâts au joueur.
🎯 Following Enemy
La classe `folowingEnemy` représente un ennemi qui suit le joueur.
Il calcule la direction entre sa position et celle du joueur, normalise cette direction puis avance vers lui.
Sa rotation est également calculée avec `atan2()` afin qu'il soit orienté vers sa direction.
> Le nom `folowingEnemy` est conservé car c'est le nom utilisé dans le code.
🌊 Vagues d'ennemis
Les ennemis sont générés progressivement avec un délai entre les apparitions.
Le programme possède différentes limites :
```cpp
maxNumberOfEnemies
maxNumberOfSlidingEnemies
maxNumberOfFolowingEnemies
```
Après certaines vagues, de nouveaux types d'ennemis peuvent apparaître.
🪙 Pièce
La classe `coin` représente une pièce récupérable.
Lorsque le joueur entre en collision avec elle, son état change et le score associé aux pièces est mis à jour.
❤️ Vie
Le joueur possède une variable :
```cpp
health
```
Elle commence à `100`.
Les collisions avec les ennemis peuvent réduire cette valeur.
Lorsque la vie atteint `0`, la partie se termine.
🏆 Score
Le score global est stocké dans :
```cpp
long long score;
```
Il est affiché pendant la partie et peut être augmenté par différentes actions.
📷 Caméra
Le projet utilise `Camera2D`.
La caméra suit le joueur et possède un système de zoom.
La conversion :
```cpp
GetScreenToWorld2D()
```
permet de transformer les coordonnées de la souris à l'écran en coordonnées du monde.
C'est notamment nécessaire pour viser correctement avec l'arme.
⏱️ Delta Time
Le programme utilise :
```cpp
float dt = GetFrameTime();
```
`dt` représente le temps écoulé depuis la frame précédente.
Il permet de rendre les déplacements indépendants du nombre de FPS.
Exemple :
```cpp
position += velocity * dt;
```
🧮 Mathématiques utilisées
Le projet utilise notamment :
Distance
```cpp
sqrt(dx * dx + dy * dy)
```
Normalisation
```cpp
dx /= distance;
dy /= distance;
```
Angle
```cpp
atan2(dy, dx)
```
Collisions
Plusieurs méthodes sont utilisées selon les objets :
AABB pour certaines collisions rectangulaires ;
distance pour certaines collisions ;
SAT pour le joueur et le `slidingEnemy`.
🗂️ Structure générale
```text
Variables globales
│
├── Caméra
├── Score
├── Ennemis
├── Couleurs
│
├── calculateAngle()
│
├── Particle
├── wall
├── player
├── Ball
├── enemyA
├── slidingEnemy
├── coin
├── folowingEnemy
│
├── Fonctions de collision
├── Fonctions de particules
│
└── main()
    ├── Initialisation
    ├── Création des objets
    ├── Boucle principale
    │   ├── Entrées
    │   ├── Spawn
    │   ├── Physique
    │   ├── Collisions
    │   ├── Ennemis
    │   └── Dessin
    └── Fermeture
```
## 📚 Objectif du projet
Ce projet est avant tout un projet personnel permettant d'expérimenter la programmation C++ et les bases d'un moteur de jeu 2D.
Les principaux concepts travaillés sont :
programmation orientée objet ;
classes et objets ;
`std::vector` ;
boucle de jeu ;
delta time ;
vecteurs et normalisation ;
collisions ;
physique simple ;
gestion des entrées ;
caméra 2D ;
particules ;
gestion d'ennemis ;
score et états de jeu.
## 🚧 État du projet
Le projet est encore en développement.
Certaines parties sont expérimentales et peuvent être améliorées :
collisions ;
équilibrage des ennemis ;
physique ;
système de combat ;
animations ;
organisation du code ;
séparation du moteur et du gameplay.
Le projet n'a pas pour objectif de se présenter comme un moteur physique complet. Il sert surtout de terrain d'expérimentation pour apprendre et construire progressivement un petit moteur de jeu en C++.
fié pour expérimenter et apprendre.
