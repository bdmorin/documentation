---
description: Installer et configurer Database Monitoring pour SQL Server auto-hébergé
further_reading:
- link: /integrations/sqlserver/
  tag: Documentation
  text: Intégration SQL Server basique
- link: /database_monitoring/troubleshooting/?tab=sqlserver
  tag: Documentation
  text: Résoudre les problèmes courants
- link: https://www.datadoghq.com/blog/migrate-sql-workloads-to-azure-with-datadog/
  tag: Blog
  text: Planifier votre migration Azure pour les workloads SQL avec Datadog
- link: https://www.datadoghq.com/blog/datadog-monitoring-always-on/
  tag: Blog
  text: Surveiller vos groupes de disponibilité AlwaysOn avec la solution Database
    Monitoring Datadog
kind: documentation
title: Configuration de Database Monitoring pour SQL Server auto-hébergé
---

{{< site-region region="gov" >}}
<div class="alert alert-warning">La solution Database Monitoring n'est pas prise en charge pour ce site.</div>
{{< /site-region >}}

La solution Database Monitoring vous permet de bénéficier d'une visibilité complète sur vos bases de données Microsoft SQL Server, en exposant des métriques de requête, des échantillons de requête, des plans d'exécution, ainsi que des états, des failovers et des événements de base de données.

Pour activer la solution Database Monitoring pour votre base de données, suivez les étapes ci-dessous :

1. [Autoriser l'Agent à accéder à la base de données](#accorder-un-acces-a-l-agent)
2. [Installer l'Agent](#installer-l-agent)

## Avant de commencer

Versions de SQL Server prises en charge : 2012, 2014, 2016, 2017, 2019, 2022

{{% dbm-sqlserver-before-you-begin %}}

## Accorder un accès à l'Agent

L'Agent Datadog requiert un accès en lecture seule pour le serveur de base de données, afin de pouvoir recueillir les statistiques et requêtes.

Créez une connexion en lecture seule pour vous connecter au serveur en attribuant les autorisations requises :

{{< tabs >}}
{{% tab "SQL Server 2014 et versions ultérieures" %}}

```SQL
CREATE LOGIN datadog WITH PASSWORD = '<MOT_DE_PASSE>';
CREATE USER datadog FOR LOGIN datadog;
GRANT CONNECT ANY DATABASE to datadog;
GRANT VIEW SERVER STATE to datadog;
GRANT VIEW ANY DEFINITION to datadog;
```
{{% /tab %}}
{{% tab "SQL Server 2012" %}}

```SQL
CREATE LOGIN datadog WITH PASSWORD = '<MOT_DE_PASSE>';
CREATE USER datadog FOR LOGIN datadog;
GRANT VIEW SERVER STATE to datadog;
GRANT VIEW ANY DEFINITION to datadog;
```

Créez l'utilisateur `datadog` dans chaque base de données de l'application supplémentaire :
```SQL
USE [database_name];
CREATE USER datadog FOR LOGIN datadog;
```
{{% /tab %}}
{{< /tabs >}}

## Installer l'Agent

Il est conseillé d'installer directement l'Agent sur le host SQL Server. En effet, cette approche permet à l'Agent de recueillir de nombreuses données de télémétrie système (processeur, mémoire, disque, réseau), en plus des données de télémétrie propres à SQL Server.

**Pour les utilisateurs AlwaysOn**, l'Agent doit être installé sur un serveur distinct et connecté au cluster via l'endpoint de l'écouteur. En effet, les données sur les réplicas secondaires des groupes de disponibilité sont recueillies à partir du principal réplica. Cette méthode d'installation permet également d'assurer le fonctionnement de l'Agent en cas de failover.

{{< tabs >}}
{{% tab "Host Windows" %}}
{{% dbm-sqlserver-agent-setup-windows %}}
{{% /tab %}}
{{% tab "Host Linux" %}}
{{% dbm-sqlserver-agent-setup-linux %}}
{{% /tab %}}
{{% tab "Docker" %}}
{{% dbm-sqlserver-agent-setup-docker %}}
{{% /tab %}}
{{% tab "Kubernetes" %}}
{{% dbm-sqlserver-agent-setup-kubernetes %}}
{{% /tab %}}
{{< /tabs >}}

## Exemples de configuration de l'Agent
{{% dbm-sqlserver-agent-config-examples %}}

## Pour aller plus loin

{{< partial name="whats-next/whats-next.html" >}}