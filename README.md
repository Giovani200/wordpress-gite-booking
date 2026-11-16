# Gîte : thème WordPress sur mesure et plugin de réservation

Site vitrine d'un gîte avec prise de rendez-vous en ligne, réalisé pour un **client réel** dans le cadre d'une compétition WordPress Warriors en première année.

Le défi n'était pas le visuel mais le code : **aucun constructeur de pages**. Le thème est écrit en PHP à partir de zéro, et la réservation repose sur un plugin développé spécifiquement.

## Équipe

| Partie |Team 7|
|---|---|
| Thème `gite-theme` | Giovani Kloussey |
| Plugin `appointment-plugin` | Jean David Zamblezi |

## Le plugin de réservation

Un visiteur choisit un créneau libre, réserve, et reçoit un e-mail de confirmation. L'administrateur est notifié et suit les rendez-vous depuis le tableau de bord WordPress.

- **Formulaire public** intégrable partout via le shortcode `[appointment_form]`.
- **Créneaux disponibles calculés à la volée** en AJAX, sans rechargement de page.
- **Pas de double réservation** : chaque demande est contrôlée contre les rendez-vous existants (chevauchement de plages horaires).
- **Table dédiée** `wp_appointments`, créée et mise à jour automatiquement à l'activation, indexée sur la date, le statut et l'e-mail.
- **Liste des rendez-vous** dans l'administration WordPress.
- **E-mails de confirmation** au client et à l'administrateur via PHPMailer et SMTP, configurables depuis une page de réglages.
- **Sécurité** : nonces sur les requêtes AJAX, assainissement des entrées (`sanitize_*`) et échappement des sorties (`esc_*`).

```
plugin/appointment-plugin/
├── appointment-plugin.php          point d'entrée, enregistrement des hooks
├── includes/
│   ├── class-appointment-database.php   accès à la table, validation, chevauchements
│   ├── class-appointment-public.php     shortcode, créneaux, réservation AJAX
│   ├── class-appointment-admin.php      menus et pages d'administration
│   ├── class-appointment-mailer.php     e-mails client et admin (SMTP)
│   └── class-appointment-(de)activator.php  rôles et installation
├── admin/partials/                 vues de l'administration
└── public/partials/                vue du formulaire
```

## Le thème

Thème classique écrit à la main, sans thème parent.

- **Bootstrap 5** chargé depuis un CDN, navigation responsive via un *nav walker* Bootstrap.
- **Compatible Gutenberg** : styles de blocs, alignements larges et pleine largeur, feuille de style dédiée à l'éditeur.
- **Personnalisable** depuis l'admin : logo, menus, zone de widgets, gabarit de page pleine largeur.
- **Mise en page en CSS Grid** (grilles d'articles, de galerie, de cartes) et variables CSS pour les couleurs.

## Installation

Prérequis : WordPress 6.4 ou plus, PHP 7.4 ou plus, Composer.

```bash
cp -r theme/gite-theme   <wordpress>/wp-content/themes/
cp -r plugin/appointment-plugin <wordpress>/wp-content/plugins/
cd <wordpress>/wp-content/plugins/appointment-plugin && composer install
```

Ensuite, dans l'administration WordPress :

1. Activer le thème **Gite theme** et le plugin **Appointment Plugin**.
2. Renseigner le serveur SMTP dans les réglages du plugin.
3. Ajouter le shortcode `[appointment_form]` dans une page.

## Stack

PHP · WordPress (thème classique, API des plugins, `$wpdb`, AJAX) · MySQL · PHPMailer · Composer · Bootstrap 5 · CSS Grid
