# **Première partie : à propos de WordPress**

### Qu’est-ce que WordPress ?

WordPress est un système de gestion de contenu créé le 27 mai 2003 par Matt Mullenweg et Mike Little, à partir d'un logiciel de blog abandonné appelé b2.

### WordPress est-il beaucoup utilisé ?

Oui, il est de loin le CMS le plus utilisé au monde, avec environ 33 pourcent de tous les sites web sur Internet en 2026, loin devant son concurrent Shopify à 4,7 pourcent.

### Combien coûte WordPress ?

Ça dépend de la version, WordPress propose un plan gratuit et des plans payants d'environ 7 CHF à 57 CHF par mois, voire plus pour les gros sites. WordPress.org est gratuit en tant que logiciel, mais il faut payer l'hébergement, environ 2 CHF à 24 CHF par mois, et un nom de domaine.

### Quelle est la différence entre wordpress.com et wordpress.org ? 

Wordpress.com héberge tout pour nous, mais avec des limites sur les plugins, thèmes et l'accès aux fichiers selon le plan. wordpress.org s'installe lui-même chez l'hébergeur de son choix, avec un contrôle total mais plus de gestion technique pour lui.

### Quest-ce qu'un CMS ? 

Un CMS est un logiciel qui permet de créer et gérer un site web sans avoir à écrire le code à la main. Il sépare le contenu, les textes et les images, de la présentation, le design du site, ce qui permet de publier une page depuis une simple interface d'administration au lieu de coder chaque page en HTML. WordPress est le CMS le plus utilisé au monde.



# **Deuxième partie : installation locale**


### 2.1 Installation de LAMP

**L**inux - **A**pache - **M**ariaDB - **P**hp 

```
lsb_release -a
```

(vérifie la version d'Ubuntu installée)

```
sudo apt update
sudo apt install apache2
sudo systemctl enable apache2
sudo systemctl status apache2
```
(installe le serveur web Apache et vérifie qu'il fonctionne)
```
sudo apt install php8.5 libapache2-mod-php8.5 php8.5-mysql php8.5-xml php8.5-gd php8.5-curl php8.5-mbstring php8.5-zip
php -v
sudo apt install mariadb-server mariadb-client
```
(installe PHP et ses modules nécessaires à WordPress)

---

### 2.2 Création de la base de données WordPress

```
sudo mysql
CREATE DATABASE wordpress;
CREATE USER 'wpuser'@'localhost' IDENTIFIED BY 'un_mot_de_passe_solide';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wpuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```
(Cette commande se connecte à MariaDB, crée la base de données "wordpress", crée un utilisateur "wpuser" avec un mot de passe, lui donne tous les droits sur cette base, applique les changements puis quitte la console MySQL.)

---

### 2.3 Téléchargement, configuration Apache et finalisation de l'installation WordPress

**Téléchargement et préparation des fichiers WordPress**

```
wget -O- https://wordpress.org/latest.tar.gz | sudo tar -xz -C /var/www
sudo chown -R www-data:www-data /var/www/wordpress
```

(télécharge la dernière version de WordPress et l'extrait dans le dossier web, puis donne les bons droits d'accès à Apache sur ces fichiers)

**Configuration d'Apache pour servir WordPress**

```
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/wordpress.conf
sudo nano /etc/apache2/sites-available/wordpress.conf
```

**ServerName** - ServerName localhost

**DocumentRoot** - changé de /var/www/html vers /var/www/wordpress

Ajout d'un bloc Directory juste avant </VirtualHost>, pour autoriser les fichiers .htaccess (nécessaire pour les permaliens WordPress) :

```
<Directory /var/www/wordpress/>
        AllowOverride All
</Directory>
```

(modification de DocumentRoot en /var/www/wordpress et ServerName en localhost)

```
sudo a2ensite wordpress.conf 
sudo a2dissite 000-default.conf
sudo a2enmod rewrite
sudo systemctl reload apache2
```

(création d'un fichier de configuration dédié pointant vers le dossier de WordPress, puis activation du site)

**Finalisation via le navigateur**

(accèder à **http://localhost/** pour suivre l'assistant d'installation de WordPress avec les identifiants de la base de données créés précédemment dans MariaDB)

---

### 2.4 Références utilisées

[LinuxConfig - Installing WordPress on a LAMP Stack in Ubuntu 24.04](https://linuxconfig.org/installing-wordpress-on-a-lamp-stack-in-ubuntu-24-04)

[Tecmint - How to Install LAMP Stack on Ubuntu 24.04](https://www.tecmint.com/install-lamp-stack-ubuntu/)

[WordPress.org - Requirements](https://wordpress.org/about/requirements/)

---

### 2.5 De quoi WordPress a-t-il besoin pour fonctionner ?

PHP, une base de données MySQL ou MariaDB, un serveur web comme Apache, et idéalement une connexion HTTPS

---

### Que sont LAMP, MAMP, WAMP et XAMPP ?

**LAMP** (Linux, Apache, MySQL/MariaDB, PHP) : la combinaison installée dans mon cas, sur Linux, en installant chaque brique séparément avec apt.

**WAMP** (Windows, Apache, MySQL, PHP) : le même principe, mais sur Windows. installable avec WampServer, qui installe Apache, MySQL et PHP en une seule fois avec une interface graphique.

**MAMP** (macOS, Apache, MySQL, PHP) : la même chose pour Mac, avec l'application MAMP, aussi tout-en-un.

**XAMPP** (X = multiplateforme, Apache, MySQL/MariaDB, PHP) : une version qui fonctionne sur Windows, Mac ET Linux à la fois.

# Troisième partie : installation distante

## Procédure d’installation de WordPress sur une VM distante

### 3.1 Installation de LAMP

**L**inux - **A**pache - **M**ariaDB - **P**hp 

```
lsb_release -a
```

(vérifie la version d'Ubuntu installée)

```
sudo apt update
sudo apt install apache2
sudo systemctl enable apache2
sudo systemctl status apache2
```
(installe le serveur web Apache et vérifie qu'il fonctionne)
```
sudo apt install php8.5 libapache2-mod-php8.5 php8.5-mysql php8.5-xml php8.5-gd php8.5-curl php8.5-mbstring php8.5-zip
php -v
sudo apt install mariadb-server mariadb-client
```
(installe PHP et ses modules nécessaires à WordPress)

---

### 3.2 Création de la base de données WordPress

```
sudo mysql
CREATE DATABASE wordpress;
CREATE USER 'wpuser'@'localhost' IDENTIFIED BY 'un_mot_de_passe_solide';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wpuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```
(Cette commande se connecte à MariaDB, crée la base de données "wordpress", crée un utilisateur "wpuser" avec un mot de passe, lui donne tous les droits sur cette base, applique les changements puis quitte la console MySQL.)

---

### 3.3 Téléchargement, configuration Apache et finalisation de l'installation WordPress

**Téléchargement et préparation des fichiers WordPress**

```
wget -O- https://wordpress.org/latest.tar.gz | sudo tar -xz -C /var/www
sudo chown -R www-data:www-data /var/www/wordpress
```

(télécharge la dernière version de WordPress et l'extrait dans le dossier web, puis donne les bons droits d'accès à Apache sur ces fichiers)

**Configuration d'Apache pour servir WordPress**

```
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/wordpress.conf
sudo nano /etc/apache2/sites-available/wordpress.conf
```

**ServerName** - ServerName 86.119.28.233 (Ip de votre VM distante)

**DocumentRoot** - changé de /var/www/html vers /var/www/wordpress

Ajout d'un bloc Directory juste avant </VirtualHost>, pour autoriser les fichiers .htaccess (nécessaire pour les permaliens WordPress) :

```
<Directory /var/www/wordpress/>
        AllowOverride All
</Directory>
```

(modification de DocumentRoot en /var/www/wordpress et ServerName en localhost)

```
sudo a2ensite wordpress.conf 
sudo a2dissite 000-default.conf
sudo a2enmod rewrite
sudo systemctl reload apache2
```

(création d'un fichier de configuration dédié pointant vers le dossier de WordPress, puis activation du site)

**Finalisation via le navigateur**

(accèder à **http://86.119.28.233/ (Ip de votre VM distante)** pour suivre l'assistant d'installation de WordPress avec les identifiants de la base de données créés précédemment dans MariaDB)

![Screensite](images/img_site.png)
