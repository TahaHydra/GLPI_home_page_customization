# Personnalisation de GLPI – Support 

Ce projet documente les personnalisations effectuées sur une instance GLPI auto-hébergée, dans le cadre du déploiement de la plateforme de support . Il inclut des ajustements fonctionnels et visuels pour adapter GLPI à une utilisation professionnelle interne.

---

## 1. Modification des statuts (`src/Ticket.php`)

**Fichier modifié :** `/var/www/html/glpi/src/Ticket.php`

### Objectifs :
- Supprimer l'affichage du statut "Processing (planned)" tout en gardant sa logique en backend, pour ne pas perturber le fonctionnement ou les transitions automatiques.
- Ne plus afficher le bloc qui contient les tickets supprimés dans l'interface utilisateur centrale.

### Détails du code :

#### Partie statuts (affichage dans les listes ou filtres)
```php
$tab = [
   self::INCOMING => _x('status', 'New'),
   self::ASSIGNED => _x('status', 'Processing (assigned)'),
   //self::PLANNED  => _x('status', 'Processing (planned)'), // masqué volontairement
   self::WAITING  => __('Pending'),
   self::SOLVED   => _x('status', 'Solved'),
   self::CLOSED   => _x('status', 'Closed')
];
```

#### Partie affichage de tickets supprimés (dans un widget ou tableau de bord central)
```php
//$twig_params['items'][] = [
//   'link'   => self::getSearchURL() . "?" . Toolbox::append_params($options),
//   'text'   => __('Deleted'),
//   'icon'   => 'fas fa-trash bg-red-lt',
//   'count'  => $number_deleted
//];
```
> Ces lignes sont commentées pour éviter d’afficher ce lien sans supprimer la logique backend associée.

---

## 2. Traduction : "Title" devient "Sujet"

**Fichier modifié :** `/var/www/html/glpi/locales/fr_FR.po`

### Objectif :
Modifier les libellés de l'interface utilisateur pour remplacer "Title" (titre du ticket) par "Sujet", ce qui correspond mieux au vocabulaire d'entreprise ou de gestion de support.

### Ligne modifiée :
```po
msgid "Title"
msgstr "Sujet"
```

### Compilation du fichier `.po` en `.mo` :
GLPI utilise les fichiers `.mo` compilés pour appliquer les traductions. Après modification du fichier `.po`, il faut le compiler avec :

```bash
msgfmt -o /var/www/html/glpi/locales/fr_FR.mo /var/www/html/glpi/locales/fr_FR.po
```

---

## 3. Nettoyage du cache et rechargement du serveur

### Objectif :
Appliquer les modifications sans attendre que GLPI le fasse automatiquement.

### Vider le cache GLPI :
Permet d'éviter le cache de fichiers obsolètes.
```bash
rm -rf /var/www/html/glpi/files/_cache/*
```

### Recharger le serveur web :
Pour recharger la configuration ou appliquer un nouveau rendu si tu modifies le HTML ou les traductions.

#### Pour NGINX :
```bash
sudo systemctl reload nginx
```

#### Pour Apache :
```bash
sudo systemctl reload apache2
```

---

## 4. Personnalisation de la page d’accueil Self-Service

**Fichier modifié :** `/var/www/html/glpi/templates/pages/self-service/home.html.twig`

### Objectifs :
- Ajouter un message de bienvenue spécifique à 
- Adapter l'interface utilisateur self-service à l'image de l’entreprise
- Réorganiser les blocs et/ou remplacer le logo si besoin

### Exemple de message ajouté :
> Bienvenue sur la plateforme de support . Cette plateforme vous permet de centraliser, suivre et structurer vos demandes de manière professionnelle. Vous pouvez consulter la documentation dans les notes publiques.

### Astuces supplémentaires :
- Les images et logos sont souvent définis via CSS ou dans le thème Twig.
- Pour modifier l'apparence visuelle plus profondément, pense à surcharger les fichiers de thème ou personnaliser via `custom.css`.

---

## 5. Commandes utiles

### Compiler une traduction `.po` en `.mo` :
```bash
msgfmt -o /var/www/html/glpi/locales/fr_FR.mo /var/www/html/glpi/locales/fr_FR.po
```

### Supprimer le cache de GLPI :
```bash
rm -rf /var/www/html/glpi/files/_cache/*
```

### Recharger le serveur web (au choix selon ton stack) :
```bash
sudo systemctl reload nginx
# ou
sudo systemctl reload apache2
```

---

## Auteur

**Taha**  
