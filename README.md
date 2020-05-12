# CLI de contrôle pour l'outil Pyrrha

Un CLI permet d'exécuter un script sans interface graphique.
Il s'agit d'un script de contrôle qualité qui vérifie les annotations de l'outil [Pyrrha](https://github.com/hipster-philology/pyrrha). Le script vérifie ligne par ligne, d'une part si les annotations sont autorisées par chacun des fichiers obligatoires et d'autre part si les annotations sont autorisées ensemble (règles indiquées dans le fichier additional_rules) et si les règles doivent être ignorées (règle ignore). Le CLI renvoie un message pour signaler qu'il n'y a pas d'erreur ou, s'il y en a, il renvoie un commentaire et le numéro de la ligne où se trouve l'erreur.


## Les fichiers à fournir au CLI

Ce CLI fonctionne avec deux fichiers d'entrée, un fichier de configuration au format .yml et un fichier à contrôler au format .tsv. Ce fichier doit être composé de 4 colonnes : le token, le lemme, la POS et la morph.

## Les fichiers obligatoires

Le fichier config.yml doit nécessairement contenir le chemin de trois fichiers: 

* fichier lemma.txt
  * sous forme de liste
  * liste tous les lemmes autorisés dans l'outil
* fichier POS.txt
  * liste sur une seule ligne toutes les catégories grammaticales sous formes d'abbréviations
  * chaque chaîne de caractère est séparée par une virgule

### Le fichier morph.tsv

* liste toutes les catégories grammaticales  : genre, cas , nombre et conjugaison
* potentiellement sous deux colonnes, au moins une colonne "morph", une colonne optionnelle "POS"
* la deuxième colonne est utilisée pour valider des traits morphologiques en fonction de l'annotation de POS

#### Exemple 1

```tsv
morph	POS
Case=Nom|Numb=Sing	NOMpro,NOMcom,PROper
Case=Voc|Numb=Sing	NOMpro,NOMcom,PROper
```

#### Exemple 2

```tsv
morph
Case=Nom|Numb=Sing
Case=Voc|Numb=Sing
```

## Les fichiers optionnels

Le fichier config.yml peut aussi contenir:

- Des règles 'ignore', qui prévalent sur les autres règles et normes. Chaque règle est composée de 3 colonnes : Index de catégorie erreur, index de token et un message intelligible qui explique l'exception. Ces trois parties doivent être séparées par des :. 
- Un fichier de règle additionel. C'est un .tsv, qui est composé de 6 colonnes et contient des règles autorisant ou interdisant la combinaison des POS et MORPH.

## Forme du fichier de configuration

Le fichier config.yml doit avoir cette forme:

```yaml
allowed_lemma: "fichier.txt"
allowed_pos: "fichier.txt"
allowed_morph: "fichier.tsv"
additional_rules: "fichier.tsv"
ignore:  
  - "AllowedLemma:8:Estre5 est particulier au projet"
```

Les ignore peuvent prendre une valeur autorisée directement en deuxième position

## Pour installer le CLI:

* Installer Python 3 
* Cloner ce repository
* Installer et activer un environnement virtuel
* Ouvrir le terminal dans le dossier cloné
* Installer le CLI en faisant python setup.py install dans votre terminal
* Veillez à ce que les fichiers de configuration soit dans le dossier cloné
* Lancer le CLI en faisant pyrrha_ci  "config.yml" "fichier à contrôler" dans votre terminal

Ce script CLI a été réalisé en mars 2019 dans le cadre d'un devoir de M2 Technologies numériques appliquées à l'histoire de l'Ecole nationale des Chartes, par deux étudiantes Marie-Caroline Schmied et Emilie Blotière sous le regard avisé de leur professeur Thibault Clérice.


