Stratégie de Pairs Trading assistée par Machine Learning
Description du projet
Ce projet implémente une stratégie d'arbitrage statistique (Pairs Trading) sur les composants du S&P 500. L'objectif est d'améliorer la rentabilité d'une stratégie classique de retour à la moyenne (Mean-Reversion) en y intégrant une couche de Machine Learning (Ensemble Learning).

L'approche consiste à filtrer les signaux générés par le modèle d'Ornstein-Uhlenbeck à l'aide d'un classifieur hybride, afin de réduire les faux positifs et d'optimiser le ratio de Sharpe.

Auteurs : LEBEAU, PATHMASRI, NASSOUR

Méthodologie
Le pipeline du projet se décompose en quatre étapes principales :

1. Sélection et Prétraitement des Données
Récupération des données historiques (10 ans) via l'API yfinance pour les tickers du S&P 500.

Nettoyage des données et calcul des rendements ajustés (Adj Close).

Test de co-intégration d'Engle-Granger pour identifier les paires d'actifs dont l'écart (spread) est stationnaire.

2. Modélisation Statistique (Baseline)
Modélisation du spread comme un processus d'Ornstein-Uhlenbeck (OU).

Calcul du Z-Score et des paramètres de retour à la moyenne (Half-Life).

Définition d'une stratégie de trading "naïve" basée sur des seuils fixes de Z-Score (entrée à +/- 2.0 écarts-types).

3. Machine Learning (Filtre de Signal)
Pour améliorer la prise de décision, plusieurs modèles de classification ont été entraînés pour prédire la probabilité de convergence du spread :

Logistic Regression : Pour capturer les relations linéaires simples.

Random Forest : Pour capturer les non-linéarités et les interactions complexes entre indicateurs (Volatilité, RSI, Momentum).

Voting Classifier : Un modèle d'ensemble combinant les deux précédents via un Soft Voting pour stabiliser les prédictions et réduire la variance.

4. Backtesting et Gestion du Risque
Implémentation d'un dimensionnement de position dynamique (Smart Sizing) basé sur la conviction du modèle (probabilité prédite).

Comparaison des performances entre la stratégie OU classique et la stratégie filtrée par ML.

Analyse des métriques financières : PnL total, Ratio de Sharpe, Max Drawdown et nombre de trades.

Résultats et Performance
Les tests effectués sur l'ensemble de validation montrent que l'approche Machine Learning permet d'améliorer la qualité des trades par rapport à l'approche purement statistique :

Réduction du Drawdown : Le filtre ML évite les entrées en position lorsque la dynamique de marché est défavorable (ex: divergence structurelle).

Amélioration du Sharpe Ratio : Bien que le nombre total de trades soit réduit, la rentabilité ajustée au risque est supérieure.

Robustesse : Les tests de non-chevauchement temporel et de comparaison Train/Test confirment l'absence de sur-apprentissage (overfitting) significatif.

Structure du Code
Le notebook Machine_Learning_Pairs_Trading.ipynb suit la logique suivante :

Configuration : Imports et paramétrage de l'environnement.

Data Fetching : Téléchargement des données boursières.

Cointegration Search : Algorithme de recherche de paires.

Feature Engineering : Création des variables explicatives pour le ML.

Training & GridSearch : Optimisation des hyperparamètres (Cross-Validation).

Ensemble Modeling : Construction du Voting Classifier.

Final Backtest : Simulation de la stratégie et visualisation des résultats.

Sanity Check : Vérification de l'intégrité temporelle des données.

Installation et Utilisation
Pour reproduire les résultats, assurez-vous d'avoir installé les dépendances suivantes :
pip install numpy pandas yfinance matplotlib seaborn scikit-learn statsmodels

