# PHP : Guide rapide

## Console interactive

Un moyen utile pour faire des tests est la console interactive que fournit php. Vous trouverez certainement un paquet `php` ou `php-cli` (ou nommé de manière proche) à installer depuis votre gestionnaire de paquets pour vous fournir la commande `php`.

Sur un système Debian GNU/Linux ou Ubuntu :
```
apt install php-cli
```
Sur un système RHEL ou Fedora :
```
dnf install php-cli
```
Le shell interactif se lance avec `php -a` et se présente comme suit :
```
~ $ php -a
Interactive shell

php > 
```
*Avertissement : il ne faut pas omettre le point-virgule `;` à la fin d'une instruction.*

## Fichiers et inclusions

En général, les fichiers php doivent avoir l'extension `.php` pour être interprétés sur le serveur. Le fichier contient du texte brut.

Le code PHP peut aisément être mélangé à du code HTML par exemple, dans tous les cas, l'extension du fichier doit être `.php`. Le code PHP est délimité par une balise ouvrante `<?php` et une balise fermante `?>`.

Le code peut être commenté sur une ligne ou plusieurs.

```
$du_code_ici; // Commentaire sur une fin de ligne

/* Commentaire sur
plusieurs lignes. */
```

Pour inclure un fichier dans un autre, on peut utiliser l'instruction `include`. En cas d'échec cette instruction retourne `false` et un warning est produit mais le programme continue.

```
include 'des_trucs.php';

```
L'instruction `require` est similaire mais produit une erreur fatale : le programme s'arrête.

```
require 'mes_fonctions.php';

```
Pour éviter des problèmes d'inclusions récursives (A inclut B mais B inclut A par exemple), on peut utiliser `include_once` ou `require_once` qui éviteront d'inclure encore quelque chose qui l'a déjà été.
```
include_once 'des_trucs.php';

require_once 'mes_fonctions.php';

```
Il faut être attentif à l'encodage utilisé par ses fichiers ainsi qu'à ceux inclus. Traditionnellement on utilisait pour la langue française l'encodage iso-8859-1 (ou windows-1252) qui date de 1986 puis iso-8859-15 introduit en 1998 (rajout du symbole €). En 2016, on a tendance à utiliser l'encodage universel UTF-8. Il est conseillé d'utiliser UTF-8.

## Redirections

On peut rediriger l'utilisateur vers une autre page facilement avec un entête HTTP.
```
header('Location: page.php');
```

## echo : afficher du texte
```
echo "Bonjour";
echo $machin;
echo "J'ai ".$age." ans.";
echo 'J'ai '.$age.' ans.';
```

Le point `.` permet de réunir des chaines et des nombres ensemble : c'est la concaténation. On peut aussi afficher le contenu d'une variable à l'intérieur d'une chaine écrite via des guillemets `"…"` et non des apostrophes `'…'`.

```
php > echo "J'ai $age ans.";
J'ai 20 ans.
php > echo 'J'ai $age ans.';
J'ai $age ans.
```
## Fonctions

### Créer une fonction

```
function Addition($a, $b) {
	return $a+$b;
}
```
On peut récupérer le résultat de plusieurs manières.
```
$resultat = Addition(2, 3);
echo Addition(18, 20);
```

Une variable déclarée dans une fonction n'existe pas en dehors de la fonction.

## Structures de données

### Variables

Une variable se définit comme ceci :
```
$variable;
```
On peut y mettre des nombres entiers, des nombres à virgule, des booléens ou des chaines de caractère (du texte).
```
$toto = 5.3;
$toto = "Bonjour";
$toto = 'Bonsoir';
$toto = true;

```

On peut faire des opérations mathématiques entre variables numériques et avec des nombres, mais pas avec du texte. Typiquement, `$toto = 3+"bonjour"` est impossible.

`true` est égal à 1 et `false` est égal à zéro.
```
php > $toto = true+4;
php > echo $toto;
5
```

On a des raccourcis utiles pour modifier une variable :

| Forme longue | Raccourci |
|-|-|
|`$var=$var+1;` | `$var++;` |
|`$var=$var-1;` | `$var--;` |
|`$var=$var*5;` | `$var*=5;` |

Pour supprimer une variable : `unset($toto);`
### Array

Un `array` en PHP peut représenter une liste (on accède aux éléments par leur position) ou un tableau (on accède aux éléments par une clé).

#### Listes

```
$canards = array('Donald', 'Picsou', 'Riri', 'Foufou', 'Loulou');
```
On peut modifier des valeurs.
```
$canards[3] = 'Fifi'; // On commence à compter à zéro
```
Ou créer un tableau comme ça :
```
$amis[] = 'Grand-mère';
$amis[6] = 'Gontran';
$amis[] = 'Popop';
```
La fonction `print_r` nous permet d'afficher le contenu brut d'un `array` :
```
php > print_r($amis);
Array
(
    [0] => Grand-mère
    [6] => Gontran
    [7] => Popop
)
```
Les valeurs sont utilisables librement indépendamment :
```
echo "Le troisième canard : ".$canards[2];
```
#### Tableaux

```
$informations = array('prenom' => 'Paul',
                      'nom' => 'Bismuth',
                      'age' => 61);
```

De manière alternative :
```
$informations['prenom'] = 'Paul';
$informations['nom'] = 'Bismuth';
$informations['age'] = 61;
```
```
php > echo "Monsieur ".$informations['nom']." a ". $informations['age']." ans";
Monsieur Bismuth a 61 ans

```
*Attention : l'usage des guillements ou apostrophes pour les clés de notre exemple (qui sont des chaines de caractères) impose d'utiliser la concaténation comme ci-dessus.*

## Structures de contrôle

Une variable déclarée dans une boucle ou condition existe aussi en dehors.

### Conditions

```
Symbole | Opérateur (renvoie `true` si...)
------------------------------------------
     == | ...égalité
      < | ...inférieur
      > | ...supérieur
     <= | ...inférieur ou égal
     >= | ...supérieur ou égal
     != | ...différent
```

*Avertissement : l'opérateur `=` est un opérateur d'affectation, pour faire un test on utilise bien `==`.*

Le bloc `if` s'exécute si le test est positif (`true` renvoyé par le test), sinon on passe au bloc `elseif` suivant (on peut en mettre autant qu'on veut), et enfin si et seulement si tous les tests ont été négatifs on exécute le bloc `else`.

```
if($age < 0) {
    echo "Impossible !";
}
elseif($age < 18) {
    echo "Mineur";
}
else {
    echo "Majeur";
}
```

On peut cumuler et imbriquer les paramètres :
```
if($age <= 25 AND ($voyageur == true OR $cas_special == true)) {
    echo "Tu as droit à des réductions en TGV.";
}
```

Les mots clés `AND` et `OR` (ou inclusif) peuvent s'écrire comme en C par `&&` et `||`.

On dispose d'un inverseur `!` :
```
if(!($grand OR $moyen)) {
    echo "Tu n'es ni grand ni moyen.";
}
```

On peut tester l'existence et la non-nullité d'une variable avec `isset` :
```
if(isset($age)) {
    echo "Tu as $age ans !";
}
```
### Boucles

```
$compteur = 1;
while($compteur <= 5) {
    echo "Ça va être affiché 5 fois.";
    $compteur++;
}
```

Équivalent compact avec la boucle `for` :

```
for($compteur = 1 ; $compteur <= 5 ; $compteur++) {
    echo "Ça va être affiché 5 fois.";
}
```

#### Pour parcourir une liste de l'indice 2 à 5 :

```
$i = 2;
while ($i <= 5) {
    echo $liste[$i];
}
```
ou

```
for ($i = 2 ; $i <= 5 ; $i++) {
    echo $liste[$i];
}
```
#### Pour parcourir les valeurs d'une liste

```
foreach($amis as $i) {
   echo $i;
}
```

Au premier passage de la boucle, la variable `$i` vaudra `Donald`, au second passage elle vaudra `Picsou`, puis `Riri` et ainsi de suite jusqu'à ce que la liste soit parcourue...

#### Pour parcourir les valeurs d'un tableau

```
foreach($informations as $cle => $donnee) {
    echo "La clé $cle vaut $donnee";
}
```
## Transmission d'informations

### Formulaires
Un moyen pratique et simple pour un visiteur de transmettre des données à un programme PHP est de mettre à disposition un formulaire HTML. On pourra concevoir une page HTML qui contiendra le formulaire suivant :
```
<form>
    <input type="text" name="nom" placeholder="Nom">
    <input type="password" name="mdp" placeholder="Mot de passe">
    <input type="submit" value="OK">
</form>
```

### Requêtes GET

```
<form action="cible.php" method="GET">
```

L'information est envoyée à travers l'URL (attention au nombre limité de caractères).

```
cible.php?nom=Jean&mdp=toto
```

On récupère l'info comme ceci : `$_GET['nom']`
### Requêtes POST

L'information est envoyée à travers l'entête HTTP, on peut transmettre plus d'informations. Attention, l'utilisateur peut aussi modifier l'information.

On récupère l'info comme ceci : `$_POST['nom']`

*Il ne faut jamais faire confiance à ce que rentre l'utilisateur via GET ou POST !*

## Bases de données

### Via mysqli

*Repris depuis [http://php.net/manual/en/mysqli.examples-basic.php](http://php.net/manual/en/mysqli.examples-basic.php)*

#### Connection

```
// Connecting to and selecting a MySQL database named sakila
// Hostname: 127.0.0.1, username: your_user, password: your_pass, db: sakila

$mysqli = new mysqli('127.0.0.1', 'your_user', 'your_pass', 'sakila');

// Oh no! A connect_errno exists so the connection attempt failed!
if ($mysqli->connect_errno) {
    echo "Sorry, this website is experiencing problems.";
}
```
#### Requêtes SQL
Exécuter la requête et récupérer le résultat dans un tableau :
```
$sql = "SELECT actor_id, first_name, last_name FROM actor WHERE actor_id = $aid";
$result = $mysqli->query($sql);
```
Afficher le premier résultat (si l'on sait qu'il n'y a qu'un résultat c'est pratique) :
```
$actor = $result->fetch_assoc();
echo $actor['first_name'] . ' ' . $actor['last_name'];
```
Afficher les résultats via une boucle :
```
while ($actor = $result->fetch_assoc()) {
    echo $actor['first_name'] . ' ' . $actor['last_name'];
}
```
Connaitre le nombre de résultats (de lignes) :
```
$result->num_rows;
```
D'autres méthodes utiles existent : [http://php.net/manual/fr/mysqli-result.num-rows.php](http://php.net/manual/fr/mysqli-result.num-rows.php)

#### Requêtes préparées

On ne peut pas faire confiance à l'utilisateur. Si on injecte le résultat d'un GET ou POST directement dans la requête SQL, on s'expose à une faille de sécurité immense, l'utilisateur pourrait rentrer dans le formulaire des ordres SQL malicieux qui seraient interprétés.

```
$stmt = $mysqli->prepare("INSERT INTO CountryLanguage VALUES (?, ?, ?, ?)");

$stmt->bind_param('sssd', $code, $language, $official, $percent);
$stmt->execute();
```
Paramètres de `bind_param` : les types de chaque variable puis respectivement (= dans le même ordre) les variables à injecter :

```
Lettre | Signification
--------------------------------------
     i | Integer (nombre entier)
     d | Double (nomble à virgule)
     s | String (chaine de caractères)
```

En cas d'échec, chacune de ces fonctions renvoie `false`.

#### Lorsque c'est fini

Fermer la connection :
```
$mysqli->close();
```
## Sessions

Les sessions permettent de retenir dans un tableau des informations personnalisées concernant chaque visiteur. Contrairement aux cookies, les sessions sont stockées côté serveur : ainsi on peut faire confiance aux données qu'elles contiennent.

Initialisation : `session_start();`

Ajouter/modifier des variables de session : 
```
$_SESSION['nom'] = 'Jean';
$_SESSION['logged'] = true;
```
Supprimer une variable de session : `unset($_SESSION['toto']);`

Pour déconnecter quelqu'un, on supprime toutes ses variables de session (sinon ça attendra le timeout) :
```
session_destroy();
```
## Fichiers

Ouvrir un fichier : `$fichier = fopen('fichier.txt', 'r');`

Différents modes :

```
Code | Description
----------------------------------------------------------------
   r | Lecture seule
  r+ | Lecture et écriture (le fichier doit exister)
   a | Écriture seule (peut créer le fichier si inexistant)
  a+ | Lecture et écriture (peut créer le fichier si inexistant)
```

Pour écrire dans le fichier : `fputs($fichier, 'Bonjour');`
Pour lire une ligne : `$ligne = fgets($fichier);`;

À chaque lecture de ligne on avance la « tête de lecture » (métaphore de la bande magnétique).
Pour stocker ligne par ligne dans le tableau :
```
$tableau = array();

for($i = 0 ; $ligne = fgets($fichier) ; $i++) {
    $tableau[$i] = $ligne;
}
```

Pour fermer le fichier : `fclose($fichier);`

## MVC : Modèle vue contrôleur

Le concept du MVC est de séparer le code en trois parties :

* Le modèle : on va chercher les données
* La vue : on affiche, on met en forme les données du modèle
* Le contrôleur : c'est là qu'on relie l'affichage et les données

On peut mettre ça en forme facilement avec trois fichiers.

**modele.php :**
```
function GetActeurs() {
    $sql = "SELECT actor_id, first_name, last_name FROM actor WHERE actor_id = $aid";
    return $mysqli->query($sql);
}
```

**vue.php :**
```
echo '<html><body><table>';

if(isset($data)) {
    while ($actor = $data->fetch_assoc()) {
        echo '<tr>';
        echo '<td>'.$actor['first_name'] .'</td>';
        echo '<td>'.$actor['last_name']  .'</td>';
        echo '</tr>';
    }
}

echo '</table></body></html>';
```

**page.php :**
```
require_once('modele.php');
$data = GetActeurs();
include_once('vue.php');
```

L'utilisateur va demander le contrôleur **page.php** qui va charger les données à partir du modèle puis appeler la vue.
