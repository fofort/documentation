# OVH

## cron job

Chez OVH (particulièrement sur les hébergements mutualisés), la méthode s'appelle le Planificateur de tâches (Cron). Vous n'avez pas besoin de gérer des lignes de commande complexes, tout se fait via l'espace client.

Voici la marche à suivre étape par étape :

1. Préparer votre script
Pour qu'un script PHP soit exécutable par le système d'OVH, il est fortement recommandé d'ajouter une "ligne d'en-tête" (Shebang) tout en haut de votre fichier pour indiquer quelle version de PHP utiliser.

Exemple pour cron-mensuel.php :

PHP
#!/usr/local/bin/php
<?php
// Votre logique ici (envoi d'email, génération CSV, etc.)
mail("votre@email.com", "Rapport Mensuel", "Voici le contenu...");
?>
2. Configurer la tâche dans l'Espace Client
Connectez-vous à votre Espace Client OVHcloud.

Allez dans l'onglet Web Cloud, puis sous Hébergements, sélectionnez votre domaine.

Cliquez sur l'onglet Plus (ou le bouton "Plus +" selon votre version) et choisissez Cron.

Cliquez sur Ajouter une planification.

Étape A : Paramètres généraux
Commande à exécuter : Indiquez le chemin vers votre script à partir de la racine (ex: www/scripts/cron-mensuel.php).

Version de PHP : Choisissez la version qui correspond à votre site (ex: PHP 8.1).

Logs par e-mail : Je vous conseille de mettre votre adresse pour recevoir un mail si le script plante.

Étape B : Fréquence (C'est ici qu'on règle le "mensuel")
Passez en Mode Expert (ou Avancé) si vous voulez une précision totale, ou utilisez le mode simple.

Pour un envoi le 1er de chaque mois à 08:00 du matin, les réglages sont :

Minutes : 0

Heures : 8

Jour du mois : 1

Mois : * (Tous les mois)

Jour de la semaine : *

3. Points de vigilance importants
Le chemin des fichiers : Dans un script Cron, les chemins relatifs ne fonctionnent pas toujours. Utilisez __DIR__ pour vos inclusions :
include(__DIR__ . '/config.php');

Limite de temps : Sur un mutualisé, OVH limite souvent l'exécution d'un Cron à quelques minutes. Si votre envoi est très lourd, il faudra peut-être le découper.

Accès direct : Pour éviter que n'importe qui puisse déclencher l'envoi en devinant l'URL du fichier, placez votre script dans un dossier protégé ou en dehors du dossier www (si votre offre le permet).

Le Script : extract_and_mail.php
Ce script se connecte à votre base de données, génère un fichier CSV "à la volée" (sans encombrer votre serveur) et l'envoie en pièce jointe.

PHP
```php
<?php
// 1. Configuration (À adapter avec vos accès OVH)
$host = 'votre_base.mysql.db';
$db   = 'nom_de_la_base';
$user = 'utilisateur';
$pass = 'mot_de_passe';
$charset = 'utf8mb4';

$destinataire = "client@exemple.com";
$objet = "Rapport Mensuel Automatique - " . date('M Y');

// 2. Connexion à la base de données
$dsn = "mysql:host=$host;dbname=$db;charset=$charset";
try {
    $pdo = new PDO($dsn, $user, $pass);
} catch (\PDOException $e) {
    die("Erreur de connexion : " . $e->getMessage());
}

// 3. Extraction des données (Exemple : commandes du mois dernier)
$query = "SELECT id, client, montant, date_achat FROM commandes 
          WHERE MONTH(date_achat) = MONTH(CURRENT_DATE - INTERVAL 1 MONTH)";
$stmt = $pdo->query($query);

// 4. Création du contenu CSV en mémoire
$csvContent = "ID,Client,Montant,Date\n"; // En-têtes
while ($row = $stmt->fetch(PDO::FETCH_ASSOC)) {
    $csvContent .= implode(",", $row) . "\n";
}

// 5. Envoi de l'e-mail avec pièce jointe
$boundary = md5(time());
$headers = "From: Robot Site Web <no-reply@votre-domaine.com>\r\n";
$headers .= "MIME-Version: 1.0\r\n";
$headers .= "Content-Type: multipart/mixed; boundary=\"$boundary\"\r\n";

$filename = "rapport_" . date('Y-m-d') . ".csv";

// Corps du message
$body = "--$boundary\r\n";
$body .= "Content-Type: text/plain; charset=utf-8\r\n\r\n";
$body .= "Bonjour,\n\nVeuillez trouver ci-joint le rapport mensuel automatique.\r\n";

// Pièce jointe
$body .= "--$boundary\r\n";
$body .= "Content-Type: text/csv; name=\"$filename\"\r\n";
$body .= "Content-Disposition: attachment; filename=\"$filename\"\r\n";
$body .= "Content-Transfer-Encoding: base64\r\n\r\n";
$body .= chunk_split(base64_encode($csvContent)) . "\r\n";
$body .= "--$boundary--";

if (mail($destinataire, $objet, $body, $headers)) {
    echo "E-mail envoyé avec succès.";
} else {
    echo "Échec de l'envoi.";
}
```
Pourquoi cette méthode est la meilleure chez OVH ?
Mémoire optimisée : On ne crée pas de fichier physique sur le disque du serveur, ce qui évite les problèmes de permissions d'écriture.

Format Standard : Le CSV généré est compatible avec Excel et Google Sheets.

Automatisation Cron : Comme expliqué précédemment, vous placez ce fichier dans votre dossier www et vous configurez la tâche Cron dans votre interface OVH.

Rappel pour la sécurité chez OVH
Si vous mettez ce fichier dans votre dossier public (www), n'importe qui peut le deviner et vider votre base de données par e-mail.

Astuce : Donnez-lui un nom complexe comme rapport_mensuel_789_xyz.php.

Mieux : Ajoutez une vérification au début du script pour qu'il ne s'exécute que si une clé secrète est présente dans l'URL, ou vérifiez si le script est lancé par le système (CLI) et non par un navigateur.

Voici comment sécuriser le script en ajoutant une Clé API (un jeton secret) que seul votre Cron OVH connaîtra.

Le Script Sécurisé : rapport_prive_xyz123.php
Remplacez la partie supérieure de votre script précédent par ce bloc de code :

PHP
```php
<?php
// 1. SÉCURITÉ : Définissez votre propre clé complexe ici
$secret_key = "MON_MOT_DE_PASSE_ULTRA_SECRET_99";

// Vérification si le script est lancé par le navigateur ou par le système (CLI)
$is_cli = (php_sapi_name() === 'cli');
$provided_key = $_GET['key'] ?? '';

// Si ce n'est pas le système ET que la clé est mauvaise -> STOP
if (!$is_cli && $provided_key !== $secret_key) {
    header('HTTP/1.0 403 Forbidden');
    die("Accès refusé : Clé API invalide.");
}

// --- SUITE DU SCRIPT (Connexion PDO et Envoi Email) ---
// ... (Insérez ici le code de connexion et d'envoi précédent)
```
Comment configurer cela dans l'espace client OVH ?
Lorsque vous allez ajouter votre tâche Cron dans le manager OVH, vous devez modifier la ligne de commande pour inclure votre clé.

Dans le champ "Commande à exécuter" :
Au lieu de mettre simplement www/rapport.php, ajoutez le paramètre à la fin :
www/rapport.php?key=MON_MOT_DE_PASSE_ULTRA_SECRET_99

Mode de fonctionnement :
Le script vérifiera si la clé passée dans l'URL correspond à celle inscrite dans le code. Si quelqu'un essaie d'accéder à votre-site.com/rapport.php, il recevra une erreur 403 Forbidden.

Bonus : Améliorer le CSV pour Excel
Si vos clients utilisent Excel, les accents français (é, à, è) peuvent mal s'afficher dans le CSV à cause de l'encodage. Pour régler cela, ajoutez ce qu'on appelle un BOM UTF-8 au début de votre contenu CSV :

PHP
```php
// Avant la boucle de données, remplacez la ligne de création :
$csvContent = "\xEF\xBB\xBF"; // Ajoute le BOM UTF-8 pour Excel
$csvContent .= "ID,Client,Montant,Date\n";
```
Résumé de votre installation :
Fichier : Placé dans un sous-dossier de /www/.

Sécurité : Clé API intégrée au script.

Automatisation : Planifié via le "Cron" du manager Web Cloud OVH.

Fréquence : Le 1er du mois à l'heure de votre choix.
