# GLPI_home_page_customization
# 📌 Personnalisation de GLPI – Support EXCO

Ce dépôt documente les modifications apportées à une instance GLPI auto-hébergée dans le cadre du projet Support EXCO. Il inclut des ajustements de l’interface, des statuts, des traductions, et de la page d’accueil self-service.

---

## ✏️ 1. Modification des statuts des tickets (`src/Ticket.php`)

**Fichier modifié :**  
`/var/www/html/glpi/src/Ticket.php`

### ✅ Objectif :
- Supprimer l'affichage du statut **"Processing (planned)"** sans casser le code
- Ne pas afficher les tickets **supprimés**

### 🔍 Détail du code modifié :

```php
$tab = [
   self::INCOMING => _x('status', 'New'),
   self::ASSIGNED => _x('status', 'Processing (assigned)'),
   //self::PLANNED  => _x('status', 'Processing (planned)'), ← supprimé de l'affichage
   self::WAITING  => __('Pending'),
   self::SOLVED   => _x('status', 'Solved'),
   self::CLOSED   => _x('status', 'Closed')
];
Suppression de l'affichage des tickets supprimés :
php
Copier
Modifier
//$twig_params['items'][] = [
//   'link'   => self::getSearchURL() . "?" . Toolbox::append_params($options),
//   'text'   => __('Deleted'),
//   'icon'   => 'fas fa-trash bg-red-lt',
//   'count'  => $number_deleted
//];
🌐 2. Traduction : Changer "Title" en "Sujet"
Fichier modifié :
/var/www/html/glpi/locales/fr_FR.po

📝 Modification :
po
Copier
Modifier
msgid "Title"
msgstr "Sujet"
🔁 Compiler le fichier .po en .mo :
bash
Copier
Modifier
msgfmt -o /var/www/html/glpi/locales/fr_FR.mo /var/www/html/glpi/locales/fr_FR.po
🔄 3. Vider le cache et recharger le serveur web
🔥 Vider le cache GLPI :
bash
Copier
Modifier
rm -rf /var/www/html/glpi/files/_cache/*
♻️ Recharger NGINX ou Apache :
Pour NGINX :

bash
Copier
Modifier
sudo systemctl reload nginx
Pour Apache :

bash
Copier
Modifier
sudo systemctl reload apache2
🖼️ 4. Personnalisation de la page d’accueil Self-Service
Fichier modifié :
/var/www/html/glpi/templates/pages/self-service/home.html.twig

✅ Objectif :
Modifier l'apparence de la page d’accueil pour les utilisateurs Self-Service

Ajouter un message de bienvenue personnalisé

Changer la structure et le logo si besoin

🧰 Commandes utiles
bash
Copier
Modifier
# Compiler les traductions
msgfmt -o /var/www/html/glpi/locales/fr_FR.mo /var/www/html/glpi/locales/fr_FR.po

# Vider le cache GLPI
rm -rf /var/www/html/glpi/files/_cache/*

# Recharger le serveur web (selon votre stack)
sudo systemctl reload nginx
sudo systemctl reload apache2
✍️ Auteur
Taha Laachari
Support IT / Déploiement GLPI pour EXCO

pgsql
Copier
Modifier

Dis-moi si tu veux une version anglaise ou une capture d’écran pour illustrer dans GitHub.
