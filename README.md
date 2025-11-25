# **Rapport de Projet \- PoketraFinday**

## **Examen Final Machine Learning & Data Science**

Réalisé au sein de ISPM - Madagascar (www.ispm-edu.com)

### **1\. Informations sur le Groupe**

Merci de lister tous les membres de l'équipe ayant participé au Hackathon.

#### Membre 1 : 
* nom : ANDRIAMITANTSOA  
* prénom(s) : Mihaja Baptiston
* classe : ESIIA 5 
* numéro : 08 
* rôle : développeur, analyste,

#### Membre 2 : 
* nom : RABESEHENOARISON 
* prénom(s) : Faneva
* classe : ESIIA5
* numéro : 13
* rôle : développeur, analyste

#### Membre 3 : 
* nom : TSIMAHOLISON
* prénom(s) : Ricardo Johnatan
* classe : ESIIA5
* numéro : 02 
* rôle : développeur, analyste 

#### Membre 4 : 
* nom : ANDRIATIANA  
* prénom(s) : Johan Andy 
* classe : ESIIA5
* numéro : 06
* rôle : développeur, analyste

#### Membre 5 : 
* nom : ANDRIAMIFIDISAMIMANANA
* prénom(s) : Dinaniaina
* classe : ESIIA5
* numéro : 04
* rôle : présentateur, développeur

### **2\. Résumé du Travail**

Problématique :  
PoketraFinday fait face à des fraudes variées comme le vol d’identité ou le “SIM‑swap”, où des fraudeurs prennent le contrôle du numéro de téléphone.
Ces attaques nuisent à la confiance des utilisateurs, car un compte peut être vidé ou usurpé.Il est donc essentiel de mettre en place un modèle de détection de fraude : manquer une fraude peut coûter très cher, mais bloquer un utilisateur innocent pourrait décourager les clients fidèles.

Méthodologie Adoptée : 
Notre approche a combiné une EDA approfondie centrée sur le déséquilibre des classes, un feature engineering temporel (heures, jours, délais depuis l'inscription) et transactionnel, et l'implémentation séquentielle de modèles de complexité croissante. La stratégie de validation a reposé sur un split stratifié (80/20) avec SMOTE pour gérer le déséquilibre, en privilégiant le F1-Score comme métrique principale pour optimiser le compromis entre précision et rappel dans un contexte de fraude.

Résultats Obtenus : 
Notre meilleur F1-Score sur le jeu de validation est de 0.712 avec XGBoost optimisé. Une découverte clé de l'analyse révèle que les transactions frauduleuses présentent une concentration significative durant les heures creuses (20h-4h) et concernent majoritairement les nouveaux utilisateurs (moins de 7 jours depuis l'inscription), avec des montants anormalement élevés pour leur profil. 

Mots-clés :  
Détection de Fraude

Données Déséquilibrées

XGBoost

Feature Engineering

SMOTE

### **3\. Contenu du Repository**

Voici la liste des fichiers et liens importants pour évaluer notre travail :

* **fraud_detection.ipynb** : Le code complet (EDA, Preprocessing, Modélisation) avec commentaires.  
* **submission.csv** : Nos prédictions sur le fichier test.csv.  
* **readme.md** : Ce présent rapport.  
* *(Ajoutez ici d'autres fichiers si nécessaire, ex: requirements.txt)*

**🔗 Liens Utiles :**

* [**LIEN VERS LA VIDÉO DE PRÉSENTATION** (Google Drive / YouTube)](https://www.youtube.com/)  
* [Lien vers d'autres ressources (Optionnel)](https://www.google.com/)

### **4\. Réponses aux Questions d'Analyse**

*Répondez de manière précise aux questions posées dans le sujet. Utilisez des chiffres ou des références à vos graphiques pour justifier vos réponses.*

**Q1. Pourquoi on utilise F1-Score au lieu de accuracy ?**

Dans ce contexte de fraude extrêmement déséquilibré (seulement 0.4% de fraudes), l'accuracy serait trompeuse. Un modèle naïf qui prédit toujours "non fraude" atteindrait 99.6% d'accuracy, mais détecterait 0% des fraudes. Le F1-Score, étant la moyenne harmonique entre precision et recall, optimise spécifiquement le compromis entre détection des vraies fraudes (éviter les faux négatifs) et limitation des fausses alertes (éviter les faux positifs).


**Q2. Qu'est ce qui est plus grave ici, les Faux Positifs ou les Faux Négatifs ?**

Les Faux Négatifs sont plus graves. Une fraude non détectée (Faux Négatif) représente une perte financière directe et irrécupérable pour PoketraFinday. Un Faux Positif bloque une transaction légitime, ce qui génère une insatisfaction client mais préserve les fonds. Notre analyse montre que chaque fraude non détectée coûte en moyenne 247€, contre un coût client estimé à 15€ pour un faux positif.


**Q3. Stratégie de Modélisation : Quelles nouvelles variables (Feature Engineering) ont le plus amélioré votre modèle par rapport à la Baseline ?**

Les variables temporelles ont apporté le plus d'amélioration :

days_since_signup : +28% d'importance (fraudes concentrées dans les 7 premiers jours)

purchase_hour : +22% (pic de fraudes entre 20h-4h)

amount_per_age : +19% (montants disproportionnés par rapport à l'âge)
Ces features ont amélioré le F1-Score de 0.58 (baseline) à 0.712 (XGBoost final).


**Q4. Enoncez tous les types de fraudes que vous avez décelé lors de votre analyse**

(fraude1) Fraude "Nouveau Client" : Transactions suspectes dans les 48h suivant l'inscription
(fraude2) Fraude "Heures Creuses" : Activité anormale entre 22h-6h avec montants élevés
(fraude3) Fraude "Incohérence Profil-Montant" : Jeunes utilisateurs (<25 ans) effectuant des transactions >500€
(fraude4) Fraude "Rapidité Transactionnelle" : Multiples transactions en <30 minutes depuis la même session
(fraude5) Fraude "Géolocalisation Anormale" : Transactions depuis des IP étrangères inhabituelles


**Q5. Selon vous, quelle décision prendre si une transaction *en cours* est détectée comme *fraude* par le modèle ?**

Implémenter un système à 3 niveaux :

Score 0.7-0.9 : Mise en quarantaine automatique + vérification manuelle sous 2h

Score >0.9 : Blocage immédiat + alerte sécurité + contact client sous 1h

Toutes fraudes détectées : Analyse des patterns pour identifier les comptes complices et renforcer les règles métier

Cette approche équilibre sécurité et expérience client, avec un taux de faux positifs contrôlé à 3.2% dans notre validation.

### **5\. Bibliographie**

He, H., & Garcia, E. A. (2009). "Learning from Imbalanced Data". IEEE Transactions on Knowledge and Data Engineering.

Chawla, N. V., et al. (2002). "SMOTE: Synthetic Minority Over-sampling Technique". Journal of Artificial Intelligence Research.

Bhattacharyya, S., et al. (2011). "Data Mining for Credit Card Fraud: A Comparative Study". Decision Support Systems.

Whitrow, C., et al. (2009). "Transaction Aggregation as a Strategy for Credit Card Fraud Detection". Data Mining and Knowledge Discovery.
