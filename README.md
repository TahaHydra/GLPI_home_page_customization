# Personnalisation de GLPI – Support EXCO

Ce projet documente les modifications appliquées à GLPI dans le cadre du déploiement de la plateforme de support EXCO. Il comprend des ajustements sur l'affichage des statuts, la traduction, la page d’accueil self-service et les droits d’accès.

---

## 1. Modification des statuts (`src/Ticket.php`)

**Fichier :** `/var/www/html/glpi/src/Ticket.php`

### Objectif :
- Masquer le statut "Processing (planned)" sans le supprimer du backend
- Retirer l’affichage des tickets supprimés dans la liste

### Code modifié :

```php
$tab = [
   self::INCOMING => _x('status', 'New'),
   self::ASSIGNED => _x('status', 'Processing (assigned)'),
   //self::PLANNED  => _x('status', 'Processing (planned)'), ← masqué
   self::WAITING  => __('Pending'),
   self::SOLVED   => _x('status', 'Solved'),
   self::CLOSED   => _x('status', 'Closed')
];
```

Suppression du lien vers les tickets supprimés :

```php
//$twig_params['items'][] = [
//   'link'   => self::getSearchURL() . "?" . Toolbox::append_params($options),
//   'text'   => __('Deleted'),
//   'icon'   => 'fas fa-trash bg-red-lt',
//   'count'  => $number_deleted
//];
```

---

## 2. Traduction : "Title" devient "Sujet"

**Fichier :** `/var/www/html/glpi/locales/fr_FR.po`

### Modification appliquée :

```po
msgid "Title"
msgstr "Sujet"
```

### Compilation du fichier `.po` en `.mo` :

```bash
msgfmt -o /var/www/html/glpi/locales/fr_FR.mo /var/www/html/glpi/locales/fr_FR.po
```

---

## 3. Nettoyage du cache et rechargement du serveur

### Vider le cache de GLPI :

```bash
rm -rf /var/www/html/glpi/files/_cache/*
```

### Recharger le serveur web :

**Pour NGINX :**
```bash
sudo systemctl reload nginx
```

**Pour Apache :**
```bash
sudo systemctl reload apache2
```

---

## 4. Personnalisation de la page d’accueil Self-Service

**Fichier :** `/var/www/html/glpi/templates/pages/self-service/home.html.twig`

### Objectifs :
- Ajouter un message de bienvenue personnalisé :
  > Bienvenue sur la plateforme de support EXCO. Cette plateforme vous permet de centraliser, suivre et structurer vos demandes de manière professionnelle.
- Modifier la disposition de la page
- Changer ou retirer le logo
- Réorganiser visuellement l’accueil pour les comptes self-service

---

## 5. Commandes utiles

```bash
# Compiler les traductions (PO → MO)
msgfmt -o /var/www/html/glpi/locales/fr_FR.mo /var/www/html/glpi/locales/fr_FR.po

# Supprimer le cache de GLPI
rm -rf /var/www/html/glpi/files/_cache/*

# Recharger le serveur web
sudo systemctl reload nginx
# ou
sudo systemctl reload apache2
```

---

## Auteur

**Taha**  
