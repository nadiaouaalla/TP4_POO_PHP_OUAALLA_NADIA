TP4 – Programmation Orientée Objet en PHP

Partie 0 – La première classe

0.3 Les objets partagent-ils la même valeur de marque ?

Non, les objets "$v1" et "$v2" ne partagent pas la même valeur de "$marque", car chaque objet possède ses propres propriétés. Modifier la marque de "$v1" ne modifie pas celle de "$v2".

---

Partie 1 – Visibilité et encapsulation

1.1 Que remarquez-vous ? Quelle ligne provoque quelle erreur ?

- "$p->getName()" fonctionne et retourne le nom complet.
- "$p->nom" fonctionne car l'attribut est public.
- "$p->cin" provoque une erreur car l'attribut est privé.
- "$p->dateNaissance" provoque une erreur car l'attribut est privé.
- "$p->getAge()" provoque une erreur car la méthode est privée.

1.2 Pourquoi serait-il dangereux de laisser "cin" en public ?

Il serait dangereux de laisser "cin" en public car toute personne pourrait accéder à cette information personnelle et la modifier directement. Il est préférable de protéger cette donnée avec "private".

1.3 "getAge()" est privée : est-elle donc inutile ?

Non, elle n'est pas inutile. Une méthode privée peut être appelée par une autre méthode de la même classe, par exemple "__toString()".

1.4 Changez la visibilité de "cin" en protected. Le comportement depuis l'extérieur change-t-il ?

Non, le comportement depuis l'extérieur ne change pas. L'attribut "protected" reste inaccessible directement depuis l'extérieur de la classe. Il devient accessible dans les classes filles.

---

Partie 2 – Constructeurs et destructeur

2.1.a Que se passe-t-il avec "new Voiture("Noire")" et "new Voiture(120)" ?

- "new Voiture("Noire")" : la couleur devient « Noire » et la vitesse conserve la valeur par défaut 0.
- "new Voiture(120)" : une erreur de type est provoquée, car le premier paramètre doit être une chaîne de caractères et non un entier.

2.1.b Quel est l'avantage de la promotion de propriétés ?

La promotion de propriétés permet de déclarer et d'initialiser les attributs directement dans le constructeur. Elle rend le code plus court, plus simple et plus lisible.

2.2.a Que remarquez-vous avec deux constructeurs ?

PHP ne permet pas de définir deux méthodes portant le même nom dans une classe. La déclaration du deuxième constructeur provoque une erreur.

2.2.b Proposez une solution avec un seul constructeur.

On peut utiliser un seul constructeur avec des valeurs par défaut :

public function __construct(
    string $nom = "",
    string $prenom = ""
) {
    $this->nom = $nom;
    $this->prenom = $prenom;
}

Ainsi, on peut créer une personne vide ou une personne avec des informations.

2.2.c Qu'est-ce qu'une méthode statique de fabrique ?

Une méthode statique de fabrique est une méthode qui crée et retourne un objet de la classe. Elle permet de proposer différentes façons de créer un objet.

2.3.a Dans quel ordre les trois messages s'affichent-ils ?

1. Voiture Blanche créée.
2. Voiture Blanche détruite.
3. Fin du script.

2.3.b Que change-t-il sans "unset()" ?

Sans "unset()", le destructeur est appelé à la fin du script. L'ordre devient :

1. Voiture Blanche créée.
2. Fin du script.
3. Voiture Blanche détruite.

2.3.c Dans quel ordre deux objets sont-ils détruits ?

En général, les objets locaux sont détruits dans l'ordre inverse de leur création : le dernier créé est détruit en premier.

2.3.d Ajouter un destructeur dans Personne.

Le destructeur peut afficher un message avec "__CLASS__" pour indiquer la destruction de l'objet.

---

Partie 3 – Surcharge de propriétés

3.1 Complétez les quatre méthodes.

- "__set()" permet de modifier une propriété inaccessible.
- "__get()" permet de lire une propriété inaccessible.
- "__isset()" vérifie si une propriété est définie et non nulle.
- "__unset()" permet de réinitialiser une propriété.

3.2 Sans "__set()", que se passerait-il ?

Sans "__set()", l'accès à une propriété privée depuis l'extérieur provoque une erreur. Avec "__set()", PHP appelle automatiquement cette méthode pour gérer la modification.

3.3 "__set()" peut-il casser l'encapsulation ?

Oui, si la méthode autorise toutes les modifications sans contrôle. Il faut vérifier les propriétés et valider les valeurs avant de les modifier.

3.4 Comment valider le nom ?

On peut utiliser "trim()" pour supprimer les espaces et "strlen()" pour vérifier que le nom contient au moins deux caractères. Si la valeur est invalide, on lance une exception avec "InvalidArgumentException".

---

Partie 4 – Représentation textuelle : "__toString()"

4.1 Pourquoi "getAge()" fonctionne-t-elle dans "__toString()" ?

Parce que "__toString()" et "getAge()" appartiennent à la même classe. Une méthode peut accéder aux méthodes privées de sa propre classe.

4.2 Que se passe-t-il si "__toString()" retourne un entier ?

La méthode "__toString()" doit retourner une chaîne de caractères. Retourner un entier provoque une erreur de type lorsque la méthode est appelée.

4.3 Quel est l'intérêt de "__toString()" ?

Elle permet d'afficher directement un objet avec "echo" et rend le code plus simple.

Deux situations pratiques :

- Afficher les informations d'une personne.
- Afficher les informations d'un étudiant dans une boucle.

---

Partie 5 – Sérialisation

5.1 Le mot de passe figure-t-il dans la chaîne sérialisée ?

Non, le mot de passe ne figure pas dans la chaîne car il est volontairement exclu dans "__serialize()".

5.2 Que constate-t-on sans "__serialize()" ?

Sans "__serialize()", PHP sérialise les propriétés de l'objet, y compris le mot de passe privé. Cela peut représenter un risque de sécurité si les données sont stockées ou partagées sans protection.

5.3 Quelle est la valeur de "motDePasse" après "unserialize()" ?

La valeur de "motDePasse" devient une chaîne vide (""""), car elle est réinitialisée dans "__unserialize()".

Dans une vraie application, il faut utiliser un système sécurisé de gestion des mots de passe, notamment "password_hash()".

5.4 Pourquoi ne faut-il jamais utiliser "unserialize()" sur les données utilisateur ?

Parce que des données malveillantes peuvent provoquer une attaque appelée PHP Object Injection. Cette attaque peut entraîner l'exécution de comportements dangereux dans l'application.

Une alternative plus sûre pour échanger des données simples est d'utiliser "json_encode()" et "json_decode()".

---

Partie 6 – Héritage

6.1.a Ajouter une classe Chat.

La classe "Chat" hérite de "Animal" et possède une méthode "miauler()".

6.1.b Peut-on appeler "$c->miauler()" ?

Non, car "$c" est un objet de la classe "Chien". La méthode "miauler()" appartient à la classe "Chat".

6.2.a Que se passe-t-il sans "parent::__construct($nom)" ?

Le constructeur de la classe mère n'est pas appelé. La propriété "$nom" n'est donc pas initialisée par le constructeur parent.

6.2.b Pourquoi "nom" est-il protected et non private ?

Parce que "protected" permet à la classe fille "Etudiant" d'accéder à la propriété "$nom", contrairement à "private".

6.3.a Quelles sont les trois erreurs ?

1. Redéfinir la méthode "afficher()" dans une classe fille provoque une erreur car elle est déclarée "final".
2. Hériter de la classe "Outil" provoque une erreur car cette classe est déclarée "final".
3. Modifier "$p->reference" provoque une erreur car la propriété est "readonly" et ne peut plus être modifiée après son initialisation.

6.3.b Donner des exemples réels.

- "readonly" : un identifiant unique ou une date de création qui ne doit pas changer.
- "final" : une classe ou une méthode dont on veut empêcher la modification par héritage.

6.4.a Peut-on appeler "getAge()" depuis Etudiant ?

Non, car "getAge()" est privée dans "Personne". Pour permettre son utilisation dans la classe fille, il faut changer sa visibilité en "protected".

6.4.b Pourquoi "dateNaissance" est-elle inaccessible ?

Parce que "dateNaissance" est déclarée "private". Une classe fille ne peut pas accéder directement aux propriétés privées de sa classe mère.

6.4.c Quel est l'avantage de "parent::__toString()" ?

Cela permet de réutiliser le code de la classe mère sans le réécrire. Le programme est plus simple et évite la répétition du code.

---

Partie 7 – Compte bancaire

7.1 Repérez l'erreur de syntaxe.

L'expression suivante est incorrecte :

$this->montant * ($this->montant 1 * $this->interet / 100);

Il manque un opérateur entre "$this->montant" et "1".

7.2 Quelle est la bonne formule ?

La formule correcte après un an est :

$montant = $montant * (1 + $interet / 100);

7.3 L'ordre des paramètres est-il cohérent ?

Non, l'énoncé demande d'abord le montant initial puis le taux d'intérêt. Il faut donc utiliser :

public function __construct(float $montantInitial, float $interet)

Cela évite les confusions lors de la création des comptes.

7.4 Résultats après correction.

Après 10 ans :

- Compte 1 : 400 DH à 10 % = environ 1 037,50 DH.
- Compte 2 : 2 500 DH à 5 % = environ 4 072,24 DH.

---

Questions de synthèse

1. Quelle est la différence entre une classe et un objet ?

Une classe est un modèle qui définit les propriétés et les méthodes. Un objet est une instance créée à partir de cette classe.

2. Citez trois avantages de la POO.

- La réutilisation du code grâce à l'héritage.
- Une meilleure organisation du programme.
- La protection des données grâce à l'encapsulation.

3. Pourquoi déclarer les attributs en private ou protected ?

Pour protéger les données et contrôler leur accès et leur modification.

4. Quelle différence entre private et protected avec l'héritage ?

- "private" : accessible uniquement dans la classe qui le déclare.
- "protected" : accessible dans la classe qui le déclare et dans ses classes filles.

5. Une méthode magique doit-elle être public ?

En général, les méthodes magiques doivent être déclarées "public" pour être appelées automatiquement par PHP. Certaines méthodes, notamment les constructeurs et destructeurs, peuvent avoir une visibilité différente selon les besoins.