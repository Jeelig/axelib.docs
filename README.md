# axelib

Axelib Mobile BaaS API documentation. <br>This documentation best describes how axelib REST API works.

```php
https://api.axelib.com/0.1/ 
```



### Register

Cette méthode crée un nouvel utilisateur dans la table "user" liée 
au projet choisit.
```php
https://api.axelib.com/0.2/register/user
```
Les paramètres <strong>email</strong> et <strong>password</strong> sont attendu en POST.<br>
Le mot de passe est haché en MD5.<br>
La méthode renvoie une réponse success true si la création s'est bien passée et false, si une erreur s'est produite.<br>
Le champs <strong>nickname</strong> prend la valeur de la chaine devant l'@
L'utilisateur reçoit un mail, avec un lien lui proposant de valider son inscription.<br>
Des informations autre que l'email et le mot de passe peuvent êtres transmises. Elles peuvent meme être rendues obligatoires en renseignant le champs <strong>reg_fields</strong> de la table projet.<br>
Les règles du mot de passe sont portées par le champs <strong>password_rules</strong>
```js
var data = {
  "email": "your_name@provider.extension", 
  "password":"my_password" 
}
```



### login

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

### logout

Déconnecte un utilisateur connecté à une application.
Sa session est terminée, son token devient invalide.
Aucune donnée en POST



### forgotpwd

Envoie un lien par mail à l'utilisateur pour réinitialiser son mot de 
passe. L'adresse email est transmise en paramètre.
Le lien envoyé est valable pendant une durée définie par projet.



### changepwd

Le mot de passe de l'utilisateur (connecté) est mis à jour avec un 
nouveau. Les paramètres password et new_password sont attendus



### fblogin

Authentifie l'utilisateur à partir de son compte facebook, via l'API
Graph



### post

Création d'un enregistrement en fonction des clés valeurs 
envoyées. Si la valeur envoyée est un objet, un enregistrement est créé. Si la valeur est un tableau, cette méthode créera plusieurs enregistrement



### get

L'ID de l'enregistrement, ainsi que son type, doit être fournit en 
paramètre GET de la requête.
En utilisant le paramètre fields en POST de la requête, on affiche uniquement les champs voulus.



### list

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
var data = {
  "fields": "field_1, field_2, ..." 
}
var data = {
  "orderBy": "field:(asc|desc)" /* ex: "orderBy": "id:asc" */
}
var data {
  "name": "like:peter",
  "age": ">=:30", 
  "name": "soundex:peter" 
}
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


### update

En POST de la requête, uniquement les champs que l'on souhaite 
mettre à jour



### delete

L'ID de l'enregistrement, ainsi que son type, doit être fournit en 
paramètre GET de la requête.
L'enregistrement est supprimé



### count

Le nombre total des enregistrements est retourné



### file

Le fichier uploadé est sauvegardé, le service répond avec l'adresse
URL du fichier ( https://files.axelib.com/{code_projet}/ )
On peut également renseigner le champs folder pour typer le fichier et les ranger dans la même catégorie.

Si le fichier est une image, les méta données de la photo sont sauvegardées (position géostationnaire, appareil de photo…).
Des miniatures sont créées aux tailles suivantes (1024px, 480px, 150px et 75px)
Si les paramètres target_table, target_field et target_id sont renseignés, l'image est ajouté à un enregistrement existant.
Si aucun target ID, l'enregistrement est créé



### query

La requête à exécuter doit exister dans la table @Query.
Le numéro de version est passé en paramètre GET. D'autres paramètres peuvent êtres saisis en POST pour enrichir la requête



### sql

La requête SQL est passée en paramètre POST.
C'est une reqête SQL classique mis à part que le nom de chaque table est précédé de : @table.
Certaines instructions sont bloquées (delete, drop, …)

### Mail

La fonction mail permet d'envoyer un email à un utilisateur inscrit sur l'application. Le mail peut être saisi dans l'appel en POST ou alors provenir d'un template saisi dans la table @Exchange.
Si l'utilisateur est inscrit sur l'application, on fournit son ID, en GET.
Si le mail est saisit dans la reqête en POST on envoie les paramètres title et body pour contruire l'e-mail.
Si le mail est dans un template, on envoie le paramètre template en POST contenant le nom du template.
Si la personne ciblée n'est pas inscrite sur l'application mais son adresse mail connue, envoyer le paramètre email en POST.
D'autres paramètres peuvent êtres transmis en POST pour alimenter le template.



### push

L'ID de l'utilisateur destinataire de la notification doit être fournie dans le GET. Si celui-ci possède un device il reçoit une notification push sur Android ou iOS



### charge

Le paiement est effectué grace à l'API de Stripe (entreprise 
spécialisée dans le domaine du paiement sur mobile)



### deviceinit

Cette méthode prend en paramètre POST le token 
d'enregistrement push du device qui emet l'appel et le sauvegarde pour un appel utltérieur













