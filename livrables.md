# Livrables - Projet 1 : Application Web HA Multi-Tiers

## 1. Schéma d'Architecture (Description)
- **Point d'entrée** : CloudFront (CDN) avec WAF global.
- **Répartition** : ALB dans les subnets publics du VPC Multi-AZ.
- **Serveur App** : ASG EC2 dans les subnets privés (Tier 2).
- **Données** : RDS MySQL Multi-AZ et ElastiCache Redis dans les subnets privés isolés (Tier 3).
- **Sécurité** : KMS pour le chiffrement, Secrets Manager pour les credentials.

## 2. Runbook Disaster Recovery (DR)
- **Objectif de Temps de Restauration (RTO)** : < 15 minutes.
- **Objectif de Point de Récupération (RPO)** : < 5 minutes (via RDS Multi-AZ).
- **Procédure de Basculement** : 
    1. RDS bascule automatiquement en cas de panne d'AZ.
    2. L'ASG relance les instances dans les AZ saines.
    3. CloudFront redirige le trafic vers les instances opérationnelles via l'ALB.
- **Restauration de Backup** : Utiliser les Snapshots RDS et S3 Versioning pour restaurer des données supprimées accidentellement.

## 3. Rapport Well-Architected (Résumé)
- **Excellence Opérationnelle** : Infrastructure déployée via CloudFormation (IaC). CI/CD via CodePipeline.
- **Sécurité** : Chiffrement KMS At-Rest, TLS In-Transit (CloudFront/ALB), IAM Roles stricts.
- **Fiabilité** : Multi-AZ sur tous les tiers (ALB, ASG, RDS).
- **Efficacité de la Performance** : Caching CloudFront et Redis. Scaling automatique (Auto Scaling).
- **Optimisation des Coûts** : Utilisation d'instances t3.micro, Budget AWS configuré, Lifecycle S3.
