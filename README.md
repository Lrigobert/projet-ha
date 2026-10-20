# TP2 : Architecture Cloud Hybride & Odoo HA

## 🚀 Objectif
Ce projet démontre la mise en place d'un environnement hybride sécurisé par VPN (Pritunl) connectant un réseau local à AWS, avec un déploiement conteneurisé d'Odoo en Haute Disponibilité.

## 🏗️ Architecture
- **Cloud AWS** : VPC Multi-AZ, ASG EC2 (Ubuntu), RDS PostgreSQL Multi-AZ.
- **VPN Hybride** : Pritunl VPN établissant un tunnel entre le réseau local et le VPC.
- **Application** : Odoo 17.0 (Docker) déployé sur AWS et en local.
- **Base de données** : RDS PostgreSQL partagée pour assurer la cohérence des données (Master-Master applicatif).

## 📂 Contenu du projet
- `terraform/` : Fichiers du TP1 (VPC, EC2, ALB).
- `docker-compose.yml` : Déploiement d'Odoo et ses dépendances.
- `cloudformation/` : Templates pour la base de données et la sécurité.

## 🛠️ Installation
1. **Infrastructure** : Déployer le VPC et RDS via Terraform ou CloudFormation.
2. **VPN** : Configurer le serveur Pritunl sur l'instance EC2 et connecter le client local.
3. **Docker** : 
   ```bash
   docker-compose up -d
   ```
4. **Vérification** : Accéder à Odoo via l'IP de l'ALB ou l'IP locale.

## 🧪 Tests de Résilience
- Arrêt d'un nœud EC2 : L'Auto Scaling relance l'instance.
- Coupure VPN : Le nœud local est isolé mais le nœud Cloud continue de servir les clients.
- RDS Failover : Basculement transparent vers l'instance de standby.
