# axelib

Axelib Mobile BaaS API documentation. <br>This documentation best describes how axelib REST API works.

```php
https://api.axelib.com/0.1/ 
```

Axelib est un service dans le cloud disponible sous forme BaaS (Backend as a Service).
Il expose aux développeurs une middleware qui est une web API accessible depuis une application.
Axelib facilite le développement d'applications en exposant des méthodes qui agissent sur les entités du projet.
API accessible depuis des applications Web et mobiles.<br>
Axelib propose les fonctionnalités suivantes : Cloud storage, Push notifications …<br>
Axelib possède sa propre API, permettant via des services intégrés, la gestion d'un projet comprenant des entités ce qui rend la mise en place beaucoup plus facile, pour les applucations orientées Web et mobile.
<br><br>
Axelib provides functionnalities such as : <br> 

- <strong>User management</strong> : a user can register / login and manage his data model<br>
- <strong>Cloud data storage</strong> :<br>
- <strong>Mail</strong> :<br>
- <strong>Push notifications</strong><br>
- <strong>Device management</strong> :<br>
- <strong>File management</strong> :<br>
- <strong>Facebook authentication</strong> : Third party authentication with graph API<br>
- <strong>Paiement</strong> : Third party API with Stripe

### REGISTER

Cette méthode crée un nouvel utilisateur dans la table "user" liée 
au projet choisit.
```php
https://api.axelib.com/0.2/register/user
```
Les paramètres `email` et `password` sont attendu en POST.<br>
Le mot de passe est haché en MD5.<br>
La méthode renvoie une réponse success true si la création s'est bien passée et false, si une erreur s'est produite.<br>
Le champs `nickname` prend la valeur de la chaine devant l'@
L'utilisateur reçoit un mail, avec un lien lui proposant de valider son inscription.<br>
Des informations autre que l'email et le mot de passe peuvent êtres transmises. Elles peuvent meme être rendues obligatoires en renseignant le champs `reg_fields` de la table projet.<br>
Les règles du mot de passe sont portées par le champs <strong>password_rules</strong>
```js
var data = {
  "email": "your_name@provider.extension", 
  "password":"my_password" 
}
```



### LOGIN

Le login permet d'uthentifier un utilisateur enregistré dans la table
"user" à l'aide de son email et mot de passe. Ce sont les deux seuls paramètres attendus.
En cas de succès, on recoit en retour le token de l'utilisateur ainsi que toutes les données relatives à l'utilisateur
```js
https://api.axelib.com/0.2/login/user 

var data = {
  "email": "your_name@provider.extension", 
  "password":"my_password" 
}
```

### LOGOUT

Déconnecte un utilisateur connecté à une application.
Sa session est terminée, son token devient invalide.
Aucune donnée en POST
```php
https://api.axelib.com/0.2/logout/user
```


### FORGOTPWD

Envoie un lien par mail à l'utilisateur pour réinitialiser son mot de 
passe. L'adresse email est transmise en paramètre.
Le lien envoyé est valable pendant une durée définie par projet.
```php
https://api.axelib.com/0.2/forgotpwd/user

var data = {
	"email": "your_name@provider.extension" 
}
```


### CHANGEPWD

Le mot de passe de l'utilisateur (connecté) est mis à jour avec un 
nouveau. Les paramètres password et new_password sont attendus
```php
https://api.axelib.com/0.2/changepwd/user

var data = {
	"password": "current", 
	"new_password": "new" 
}
```


### FBLOGIN

Authentifie l'utilisateur à partir de son compte facebook, via l'API
Graph
```php
https://api.axelib.com/0.2/fblogin/user

var data = { 
	email:"youremail@provider.com", 
	first_name:"Test", 
	gender:"male", 
	id:"10212937671531750", 
	last_name:"KELLY", 
	link:"https://www.facebook.com/app_scoped_user_id/10212937671531750/", 
	locale:"fr_FR", 
	name:"Test KELLY", 
	timezone:2, 
	updated_time:"2017-01-22T20:07:58+0000", 
	verified:true 
}
```


### POST

Création d'un enregistrement en fonction des clés valeurs 
envoyées. Si la valeur envoyée est un objet, un enregistrement est créé. Si la valeur est un tableau, cette méthode créera plusieurs enregistrement
```php
https://api.axelib.com/0.2/post/{entity}

var data = { 
	"field_name": "field_value"
	... 
}
```


### GET

L'ID de l'enregistrement, ainsi que son type, doit être fournit en 
paramètre GET de la requête.
En utilisant le paramètre fields en POST de la requête, on affiche uniquement les champs voulus.
```php
https://api.axelib.com/0.2/get/{entity}/{ID}

var data = { 
	"fields": "field_1, field_2, ..."
}
```


### LIST

Les 10 premiers élémenst sont retournés par défaut par ordre de 
création décroissant.
On peut choisir les champs affichés en envoyant en POST fields.
If page and nb_item are given, the pagination is applied.
Filters : 
 - data comparison : >=, = soundex ...
 - data combining : Join, add, plus ...
 - ordering : orderBy
Using /all returns the whole table  (all the records)
Sending ax_full_search as POST  param searches on all fields the text given
```js
https://api.axelib.com/0.2/list/{entity}/{page}/{nb_item}
```

<strong>Fields</strong>

```js
var data = {
  "fields": "field_1, field_2, ..." 
}
```

<strong>Ordering</strong>

```js
var data = {
  "orderBy": "field:(asc|desc)" /* ex: "orderBy": "id:asc" */
}
```

<strong>Filtering</strong>

```js
var data = {
  "name": "like:peter",
  "age": ">=:30", 
  "name": "soundex:peter" 
}
```

<strong>Ordering</strong>

```js
var data = {
  "join:type,color": "user.type_id=type.id&test=1,height>=15" 
}
var data = {
  "count:like,finger": "user_id=1&test=1,height>=15" 
}
var data = {
  "exist:like,finger": "user_id=1&test=1,height>=15"
}
var data = {
  "prop:type, color": "type_id:name, color_id:color" 
}
var data = {
  "plus:name in type, name in color": "my_type:table.type_id=color.id,my_color:color_name=color.name" 
}
var data = {
  "linkedto": "car:user_id" 
}
```


### UPDATE

En POST de la requête, uniquement les champs que l'on souhaite 
mettre à jour
```php
https://api.axelib.com/0.2/update/{entity}/{ID}
```


### DELETE

L'ID de l'enregistrement, ainsi que son type, doit être fournit en 
paramètre GET de la requête.
L'enregistrement est supprimé
```php
https://api.axelib.com/0.2/delete/{entity}/{ID}
```


### COUNT

Le nombre total des enregistrements est retourné
```php
https://api.axelib.com/0.2/count/{entity}
```


### FILE

Le fichier uploadé est sauvegardé, le service répond avec l'adresse
URL du fichier ( https://files.axelib.com/{code_projet}/ )
On peut également renseigner le champs folder pour typer le fichier et les ranger dans la même catégorie.
```php
https://api.axelib.com/0.2/file/upload
```
Si le fichier est une image, les méta données de la photo sont sauvegardées (position géostationnaire, appareil de photo…).
Des miniatures sont créées aux tailles suivantes (1024px, 480px, 150px et 75px)
Si les paramètres target_table, target_field et target_id sont renseignés, l'image est ajouté à un enregistrement existant.
Si aucun target ID, l'enregistrement est créé
```php
var data = {
  file: myFile, 
  target_table: "tab_name", 
  target_field: "field_name", 
  target_id: "id", 
  "folder": "folder_name"
}
```


### QUERY

La requête à exécuter doit exister dans la table @Query.
Le numéro de version est passé en paramètre GET. D'autres paramètres peuvent êtres saisis en POST pour enrichir la requête
```php
https://api.axelib.com/0.2/query/{query_name}/{version}

var data = { 
	"param_name": "param_value", 
	... 
}
```


### SQL

La requête SQL est passée en paramètre POST.
C'est une reqête SQL classique mis à part que le nom de chaque table est précédé de : @table.
Certaines instructions sont bloquées (delete, drop, …)
```php
https://api.axelib.com/0.2/sql/query

var data = { 
	"sql": "SELECT * FROM `@table.table_name`" 
}
```


### MAIL

La fonction mail permet d'envoyer un email à un utilisateur inscrit sur l'application. Le mail peut être saisi dans l'appel en POST ou alors provenir d'un template saisi dans la table @Exchange.
Si l'utilisateur est inscrit sur l'application, on fournit son ID, en GET.
Si le mail est saisit dans la reqête en POST on envoie les paramètres title et body pour contruire l'e-mail.
Si le mail est dans un template, on envoie le paramètre template en POST contenant le nom du template.
Si la personne ciblée n'est pas inscrite sur l'application mais son adresse mail connue, envoyer le paramètre email en POST.
D'autres paramètres peuvent êtres transmis en POST pour alimenter le template.
```php
https://api.axelib.com/0.2/mail/user/{ID}

var data1 = { 
	"title": "Email title", 
	"body": "Email body here …" 
}

var data2 = { 
	"template": "transaction" 
}

var data3 = { 
	"template": "transaction", 
	"email": "my_email@provider.com"
}
```
Il est possible d'envoyer des paramètres supplémentaires. Ceux-ci sont remplacés dans le template du mail : <strong>__param__</strong>
En envoyant param en POST, cette chaine sera remplacée.
Sinon, pour lire des données sur la table utilisateur, dans le template on ajoute : <strong>__user:param__</strong>



### PUSH

L'ID de l'utilisateur destinataire de la notification doit être fournie dans le GET. Si celui-ci possède un device il reçoit une notification push sur Android ou iOS
```php
https://api.axelib.com/0.2/push/user/{ID}

var data = { 
	title: "Push title", 
	message: "Push body … " 
}
```


### CHARGE

Le paiement est effectué grace à l'API de Stripe (entreprise 
spécialisée dans le domaine du paiement sur mobile)
```php
https://api.axelib.com/0.2/charge/instance

var data = { 
	amount: total_amount, 
	token: stripe_token_card, 
	id: stripe_token_id 
}
```


### DEVICEINIT

Cette méthode prend en paramètre POST le token 
d'enregistrement push du device qui emet l'appel et le sauvegarde pour un appel utltérieur
```php
https://api.axelib.com/0.2/deviceinit/instance

var data = { 
	token: token_value 
}
```





