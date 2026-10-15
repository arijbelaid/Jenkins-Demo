# 🚀 Jenkins CI/CD Pipeline — Lab 2

[![Jenkins](https://img.shields.io/badge/Jenkins-2.580.1-red)](https://www.jenkins.io/)
[![Docker](https://img.shields.io/badge/Docker-29.1.3-blue)](https://hub.docker.com/u/arijbelaid)
[![Java](https://img.shields.io/badge/Java-21-orange)](https://openjdk.org/)
[![Maven](https://img.shields.io/badge/Maven-3.9.12-purple)](https://maven.apache.org/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-2.1.5-green)](https://spring.io/)

Pipeline **CI/CD as Code** réalisé avec **Jenkins**, **Maven**, **Docker** et un **Jenkinsfile** versionné.
Ce projet fait suite au Lab 1 et met en œuvre la méthode **Pipeline as Code** pour automatiser le build, la conteneurisation et le déploiement d'une application **Spring Boot**.

---

## 📄 Rapport complet

Le rapport détaillé du Lab 2 est disponible dans le dossier `rapport-lab2/`.

---

## 📋 Sommaire

* [1. Présentation](#1-présentation)
* [2. Architecture](#2-architecture)
* [3. Prérequis](#3-prérequis)
* [4. Partie 1 — Installation Java 21 + Jenkins](#4-partie-1--installation-java-21--jenkins)
* [5. Partie 2 — Préparation du projet](#5-partie-2--préparation-du-projet)
* [6. Partie 3 — Installation des plugins](#6-partie-3--installation-des-plugins)
* [7. Partie 4 — Configuration des outils](#7-partie-4--configuration-des-outils)
* [8. Partie 5 — Cloud Docker](#8-partie-5--cloud-docker)
* [9. Partie 6 — Création du Pipeline](#9-partie-6--création-du-pipeline)
* [10. Partie 7 — Jenkinsfile](#10-partie-7--jenkinsfile)
* [11. Partie 8 — Lancement du Pipeline](#11-partie-8--lancement-du-pipeline)
* [12. Résultats](#12-résultats)
* [13. Auteur](#13-auteur)

---

# 1. Présentation

## 🎯 Objectif

Mettre en place un **pipeline CI/CD as Code** pour une application **Spring Boot** en utilisant :

* **Jenkins** : Automatisation du pipeline
* **Jenkinsfile** : Définition du pipeline versionnée dans Git
* **Maven** : Build de l'application Java
* **Docker** : Conteneurisation
* **GitHub** : Hébergement du code et du Jenkinsfile

## 🛠️ Technologies utilisées

| Technologie | Version | Rôle |
|---|---|---|
| Ubuntu Server | 26.04 | Système d'exploitation |
| Java JDK | 21 | Runtime Java |
| Maven | 3.9.12 | Build Java |
| Docker | 29.1.3 | Conteneurisation |
| Jenkins | 2.580.1 | Automatisation CI/CD |
| Spring Boot | 2.1.5 | Application |

---

# 2. Architecture

```text
┌─────────────────────────────────────────────────────────────┐
│                    VM MASTER (192.168.159.15)                │
│                                                              │
│   ┌──────────────┐       ┌──────────────┐                   │
│   │   JENKINS    │◄──────┤  JENKINSFILE │                   │
│   │  (port 8080) │       │   (GitHub)   │                   │
│   └──────┬───────┘       └──────────────┘                   │
│          │                                                   │
│          ▼                                                   │
│   ┌──────────────┐       ┌──────────────┐                   │
│   │    MAVEN     │       │    DOCKER    │                   │
│   │   (build)    │       │  (build+run) │                   │
│   └──────────────┘       └──────┬───────┘                   │
│                                  │                           │
│                                  ▼                           │
│                        ┌─────────────────┐                  │
│                        │  CONTENEUR      │                  │
│                        │  devopsexample  │                  │
│                        │   port 2222     │                  │
│                        └─────────────────┘                  │
│                                                              │
└─────────────────────────────────────────────────────────────┘
