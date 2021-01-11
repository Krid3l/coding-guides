# Sulcata Studio - Guide de style de code PHP (en français)

Ce guide est le résultat du consensus de tous et des concessions de chacun. Nous avons tous nos manières et manies lorsque nous écrivons du code. Cela étant, il est important de disposer de bases de code ayant les qualités suivantes :
- Uniformisées
- Maintenables
- Extensibles

C'est à ces fins que ce document existe. Dans cette optique, nous vous demandons de le respecter autant que possible lorsque vous contribuez du code à nos projets. Ce guide traite exclusivement de notre utilisation du langage PHP - D'autres guides seront élaborés plus tard pour chacun des langages que nous utilisons régulièrement.  
  
Ce guide sera mis à jour régulièrement jusqu'à ce que sa complétude soit satisfaisante.
  
Merci à Rosetta Code pour certains des bouts de code illustrant ce guide.


## Sommaire

    1. Placement des accolades  
    2. Conventions de nommage
    3. Espacement et tabulations
    4. Fonctions
    5. Commentaires
    6. Tags
    7. Longueur des lignes
    8. Utilisation des guillemets
    9. Alias et capitalisation des opérateurs et mots-clés
    10. Classes
    11. Requêtes SQL


## 1. Placement des accolades  
  
Notre école est celle de l'**OTBS**, ou "One True Brace Style".
Il s'agit du classique style d'indentation où chaque bloc de code a son accolade d'ouverture sur la même ligne que l'instruction de contrôle ou déclaration de fonction :

```php  
// https://rosettacode.org/wiki/Sum_multiples_of_3_and_5#PHP
function sumMut3And5() : int {
    $max = 1000;
    $sum = 0;
    for ($i = 1; $i < $max; $i++) {
        if (($i % 3 == 0) or ($i % 5 == 0)) {
            $sum += $i;
        }
    }
    return $sum;
}
```

Lorsqu'une seule instruction est présente dans un bloc, on garde l'OTBS et on évite les one-liners :
  
```php  
// Nope
if (thing) { do_stuff(); }
  
// Nope
if (thing)
    do_stuff();
  
// OK
if (thing) {
    do_stuff();
}
  
```
  
Rationale : Le premier bout de code a une esthétique discutable, et le deuxième demande l'ajout tardif d'accolades si une deuxième instruction arrive plus tard.  
  
Ces règles n'excluent pas l'utilisation des opérateurs ternaires, dont l'usage est à l'appréciation de chacun selon la situation :  
  
```php
$cake_is_lie = glados() ? true : false;
```


## 2. Conventions de nommage  
  
De manière générale, donnez des noms descriptifs sans qu'ils soient trop longs.

```php
// Les variables et noms de fichiers sont en snake_case  
$lookout_for_long_var_names = 42;  
  
// Les identifiants de fonctions sont en camelCase  
function proveExistanceOfUnicorns() { /* ... */ }
  
// Les identifiants de classes sont en PascalCase
class DubiousProcessWorker() { /* ... */ }  
  
// Les constantes, ainsi que les variables dont la valeur ne doit pas changer, sont en MAJUSCULES_AVEC_UNDERSCORES
define('EPIC_NUMBER', 1337);
```  
  
En cas de doute, privilégier la nomenclature snake_case.


## 3. Espacement, tabulations et sauts de ligne
  
La longueur des tabulations dans tous nos projets est *systématiquement* de 4 caractères.
  
Un espace doit être inclus :

```php  
// Avant et après tout opérateur, dont voici la liste :
// https://www.php.net/manual/fr/language.operators.php
$machin_chose += 5;  

// ...sauf si c'est une incrémentation / décrémentation unitaire,
// un opérateur de contrôle d'erreur ou un opérateur d'exécution
$machin_chose++;
  
// Avant et après des parenthèses initiales
foreach (scandir('/home/foo/bar') as $file) {
    if ($file != '.' && $file != '..') {
        if (preg_match("/php/", $file)) {
            // ...
        }
    }
}  
  
// ...sauf lors de l'appel d'une fonction où l'espace prédécesseur est abandonné  
someAwesomeFunc();
```  
  
Les sauts de ligne sont tolérés au sein d'un bloc - quel qu'il soit - à des fins d'aération du code. Préférez de telles séparations entre des ensembles d'instructions. **Sauf** si c'est immédiatement après son ouverture :  
  
```php    
// Dégueu
function candyFactory() {

    premiereInstruction();
}  
  
// C'est mieux  
function candyFactory() {
    premiereInstruction();
}
```

Exemple typique de saut de ligne justifié : Avant un return, si le return n'est pas la seule instruction de la fonction. Exemple dans la section 4 (Fonctions).

Dans la mesure du possible, et si l'impact esthétique n'en est pas frappant, les groupes d'affectation devraient être alignés à partir de l'opérateur :

```php
$cursor         = 0;
$bbq_temp       = 120.0;
$ingredients    = fetchMeats();
```

Lorsque les paramètres d'une fonction sont définis ou que des arguments sont transmis lors d'un appel de fonction, mettre **uniquement un espace après les virgules** séparant les paramètres / arguments.

Pour finir, il convient de se limiter à **une instruction par ligne**.
  

## 4. Fonctions  
  
La délcaration des fonctions doit suivre, de manière générale, le schéma suivant :  

```php
public function croissantCatapult(int croissant_amount, float angle = 0.0, float force) : bool {
    echo 'Hon hon hon, it rains viennoiseries!';
    $shot_data = fireCatapult();

    return $shot_data->target_hit;
}
```  
  
Préciser le type des paramètres et des valeurs de retour *autant que possible*.

Vous pouvez déroger à ce moule lorsqu'il y a beaucoup de paramètres et / ou lorsque leurs noms sont très longs - Voir section 7 (Longueur des lignes) notamment. Dans ce cas, faites des retours à la ligne et indentez d'un espace (et non d'une tabulation) :

```php
public function croissantCatapult(
 int croissant_amount,
 float angle_of_fire,
 float force_applied,
 bool monitor_shot_efficiency,
 array params_for_next_shot) : bool {
    echo 'Hon hon hon, it rains viennoiseries!';
    $shot_data = fireCatapult();

    return $shot_data->target_hit;
}
```
  

## 5. Commentaires

Lorsque le code est annoté au sein d'une fonction ou sur la même ligne qu'une instruction, les commentaires en ligne simples ( `//` ) sont à privilégier.  
  
Si vous commentez une fonction ou que vous rédigez un court pavé explicatif, utilisez plutôt un commentaire multiligne ( `/* */` ) de style Docblock, comme illustré ci-dessous. Vous n'êtes pas obligé d'utiliser les identifiants Docblock tels que `@param` et `@return`, sauf si vous les jugez utiles pour décrire plus précisément le fonctionnement de grosses fonctions par exemple.

```php  
/**
* Fonctions variadiques en PHP
*
* Bout de code piqué de Rosetta Code :
* https://rosettacode.org/wiki/Variadic_function#PHP
*/
function printAll() {
    // première méthode
    foreach (func_get_args() as $x) {
        echo "$x\n";
    }
 
    $numargs = func_num_args(); // deuxième méthode
    for ($i = 0; $i < $numargs; $i++) {
        echo func_get_arg($i), "\n";
    }
}
  
```


## 6. Tags

*Ne jamais utiliser* les tags courts en PHP ( `<? ?>` ). Toujours utiliser les tags pleins ( `<?php ?>` ).

Toujours mettre les tags d'ouverture et de fermeture sur leur propre ligne, et avec une ligne de séparation. Garder le code PHP au même niveau d'indentation que les tags. Cela vaut aussi pour le PHP inséré dans de l'HTML.

```php  
<?php  
  
function saySomethingStupid() {
    echo 'Limoncello tastes awful.';
}

saySomethingStupid();

?>
```  
  
**Important** : Il convient de ne *jamais* mettre de tag fermant ( ?> ) lorsqu'on est à la fin d'un fichier PHP.


## 7. Longueur des lignes

En prenant en compte l'indentation, chaque ligne de code PHP **ne devrait pas faire plus de 80 caractères** de long. Cette limite, correspondant à la largeur maximale des terminaux standards récents (en caractères), est également présente dans de nombreux autres guides de style pour de nombreux langages.  
  
Il convient de la respecter afin de permettre à tout contributeur, peu importe son outil d'écriture de code, de pouvoir lire comfortablement chaque instruction.

Si besoin, tranchez l'instruction ou la déclaration avec des retours à la ligne, ou optimisez le code en décomposant davantage le traitement effectué.


## 8. Utilisation des guillemets

Les simples guillemets doivent être utilisés la plupart du temps, notamment lors de la définition de chaînes de caractères ( `'coucou'` ) ou de la définition de constantes. En revanche, dans les cas où vous avez besoin de conserver les séquences d'échappement, utilisez des doubles guillemets ( `"coucou\n"` ).  
  
  
## 9. Alias et capitalisation des opérateurs et mots-clés  
  
Tous les mots-clés de PHP utilisés dans notre code, y compris **true**, **false** et **null** doivent être écrits en minuscules.  
  
Les opérateurs logiques ne doivent pas être écrits en anglais - Préférer leurs homologues formés avec des caractères spéciaux.

```php  
// Nope
if (($cond1 or $cond2->attr) and cond3()) { /* ... */ }
  
// OK
if (($cond1 || $cond2->attr) && cond3())  { /* ... */ }
```


## 10. Classes  
  
Respectez le modèle suivant lors de la déclaration de vos classes (merci au guide de style PHP du MIT) :

```php  
<?php

class Log               /** 
class Net_Finger        * respect du modèle hiérarchique PEAR
class HTML_Upload_Error */

class AMoreCompleteExample
{
    public $counter;                // propriété publique
    function connect();             //
    function getData();             // méthodes publiques
    function buildSomeWidget();     //

    protected $query;               // propriété protégée
    protected function sendQuery(); // méthode protégée

    private $_status;               //
    private function _sort();       // méthodes privées
    private function _initTree();   //  
  
    // ... 
```  
  
Les propriétés et méthodes privées sont, par convention, précédées d'un underscore ( `_` ).


## 11. Requêtes SQL  
  
Merci là encore au guide de style PHP du MIT.

Les mots-clés SQL sont **toujours en majuscules** : SELECT, INSERT, UPDATE, WHERE, AS, JOIN, ON, IN, etc.  
  
Ne pas hésiter à séparer les requêtes longues en plusieurs lignes pour respecter la limite de 80 caractères détaillée dans la section 7 (Longueur des lignes).  
  
Il est également obligatoire d'appliquer toutes les mesures de sécurité nécessaires, telles que la préparation des requêtes, afin de prévenir toute injection SQL.
