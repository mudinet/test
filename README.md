Extraction des audits Apache Ranger via Solr
🎯 Objectif

Ce projet documente la procédure permettant d’extraire les journaux d’audit Apache Ranger depuis Apache Solr, afin de faciliter :

les audits de sécurité

les analyses de conformité

les investigations d’accès utilisateurs

Les données sont exportées au format CSV pour une exploitation simple.

🔧 Prérequis

Accès à un environnement Linux

Outil curl installé

Accès à un cluster Apache Ranger / Solr

Authentification Kerberos (SPNEGO) active

Ticket Kerberos valide

Vérification du ticket :

klist

🚀 Commande d’extraction (modèle)
curl -o ranger_audits_export.csv \
  --negotiate -u <USER> \
  -X GET "http://<SOLR_HOST>:8983/solr/ranger_audits/select?q=resource:*<APP_SCOPE>*&fq=evtTime:[<START_DATE>T00:00:00Z TO NOW]&fq=-resource:*tmp*&fq=-resource:*staging*&fq=-resource:*mr-history*&fq=-resource:*app-logs*&fq=-reqUser:<SYSTEM_USERS>&fl=policy,evtTime,reqUser,repo,resource,resType,access,result,cluster&sort=evtTime desc&start=0&rows=300000&wt=csv"

🔐 Authentification

La commande utilise l’authentification Kerberos / SPNEGO :

--negotiate -u <USER>


📌 Assurez-vous qu’un ticket Kerberos valide est présent avant l’exécution.

🔎 Filtres appliqués
Périmètre applicatif

Sélection des ressources liées à un périmètre spécifique

Période

Extraction depuis une date donnée jusqu’à la date courante

Exclusions

Ressources temporaires et techniques

Environnements de staging

Comptes systèmes

📑 Champs exportés

Les colonnes suivantes sont incluses dans le fichier CSV :

policy : politique Ranger appliquée

evtTime : date et heure de l’événement

reqUser : utilisateur demandeur

repo : service Ranger

resource : ressource accédée

resType : type de ressource

access : type d’accès

result : autorisé ou refusé

cluster : cluster concerné

📊 Tri et volumétrie

Résultats triés par date décroissante

Jusqu’à 300 000 événements par extraction

Pagination possible via les paramètres start et rows

📁 Résultat

Fichier CSV généré localement

Données prêtes pour :

analyse Excel

audits de sécurité

investigations techniques

⚠️ Bonnes pratiques

Adapter la période pour éviter des volumes trop importants

Ne pas exposer de données sensibles dans les dépôts Git

Exécuter la commande depuis un environnement sécurisé

📚 Références

Apache Ranger – Auditing
https://ranger.apache.org/

Apache Solr – Common Query Parameters
https://solr.apache.org/guide/solr/latest/query-guide/common-query-parameters.html

Apache Solr – CSV Response Writer
https://solr.apache.org/guide/solr/latest/query-guide/response-writers.html#csv-response-writer

curl – Documentation officielle
https://curl.se/docs/manpage.html

curl & Kerberos / SPNEGO
https://everything.curl.dev/http/auth/spnego

Si tu veux, je peux :

adapter le README à un standard d’entreprise

ajouter une section sécurité / conformité

fournir une version bilingue (FR/EN)
