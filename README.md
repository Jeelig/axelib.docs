# axelib

Axelib Mobile BaaS API documentation. <br>This documentation best describes how axelib REST API works.

```php
https://api.axelib.com/0.1/ 
```



### Register

Cette méthode crée un nouvel utilisateur dans la table "user" liée 
```php
https://api.axelib.com/0.2/register/user
```
au projet choisit.<br>
Les paramètres email et password sont attendu en POST.
```js
{ 
	"email": "your_name@provider.extension", 
	"password":"my_password" 
}
```
Le mot de passe est haché en MD5.<br>
La méthode renvoie une réponse success true si la création s'est bien passée et false, si une erreur s'est produite.<br>
Le champs nickname prend la valeur de la chaine devant l'@
L'utilisateur reçoit un mail, avec un lien lui proposant de valider son inscription.<br>
Des informations autre que l'email et le mot de passe peuvent êtres transmises. Elles peuvent meme être rendues obligatoires en renseignant le champs reg_fields de la table projet.<br>
Les règles du mot de passe sont portées par le champs password_rules
