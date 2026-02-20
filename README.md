# smart-mobility-config-repo

Dépôt centralisé des fichiers de configuration de la plateforme **Smart Mobility**.

## Rôle
Ce dépôt est lu par le **smart-mobility-config-server** (Spring Cloud Config Server).
Chaque microservice demande sa configuration au Config Server au démarrage ;
celui-ci la lit ici et la renvoie au service.

## Structure des fichiers

| Fichier | Service concerné | Port |
|---|---|---|
| `application.yml` | Valeurs partagées par **tous** les services | — |
| `eureka-mobility-service-registry.yml` | Service Registry (Eureka) | 8761 |
| `smart-mobility-user-service.yml` | User & Mobility Pass Service | 8081 |
| `smart-mobility-trip-service.yml` | Trip Management Service | 8082 |
| `smart-mobility-pricing-service.yml` | Pricing & Discount Service | 8083 |
| `smart-mobility-billing-service.yml` | Billing Service | 8084 |
| `smart-mobility-notification-service.yml` | Notification Service | 8085 |
| `smart-mobility-api-gateway.yml` | API Gateway | 8765 |

## Convention de nommage
Spring Cloud Config résout les fichiers dans l'ordre suivant (du plus spécifique au plus général) :
```
{application}-{profile}.yml  →  {application}.yml  →  application-{profile}.yml  →  application.yml
```
Le fichier `application.yml` contient les **valeurs par défaut** héritées par tous les services.

## Comment modifier une configuration ?
1. Éditez le fichier YAML du service concerné.
2. Committez et poussez la modification dans ce dépôt.
3. Appelez l'endpoint `/actuator/refresh` sur le service cible
   (ou redémarrez-le) pour recharger la configuration à chaud.

## Vérification rapide
Après avoir démarré le Config Server (port 9999), interrogez :
```
GET http://localhost:9999/eureka-mobility-service-registry/default
GET http://localhost:9999/smart-mobility-user-service/default
```
Vous devez obtenir un JSON contenant les propriétés du fichier YAML correspondant.
