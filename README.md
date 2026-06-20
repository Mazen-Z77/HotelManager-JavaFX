# HotelManager — Application de gestion hôtelière

Application de bureau **JavaFX** permettant de gérer les **chambres** et les
**réservations** d'un hôtel, avec persistance dans une base de données
**MySQL** via **JDBC**. Le projet suit une architecture **MVC / DAO** claire,
où chaque fichier a un rôle unique et bien défini.

> Mini-projet — Module *Développement Java IHM* • ENSAO • Filière **GI3** •
> Année universitaire **2025-2026** • Encadrante : **Mme Douae EL HILA**.

Consulter le Rapport de Projet (PDF)
Visionner la Démonstration Vidéo : https://drive.google.com/file/d/1YrTv9qJOYLXgQ5RI23Xc1SadWFOLL-GK/view?usp=sharing
---

##  Ce qui rend cette application unique

- **Interface premium « Maison Azur »** : barre latérale de navigation à dégradé,
  en-tête raffiné, cartes à ombrage et coins arrondis.
- ** Mode clair / sombre** : bascule instantanée depuis l'en-tête ou le menu,
  toute l'application change de palette.
- ** Accent dynamique cohérent** : la couleur choisie au tableau de bord
  recolore aussi la barre latérale et les graphiques (looked-up colors CSS).
- ** En-tête vivant** : horloge en temps réel, date et salutation contextuelle
  (Bonjour / Bon après-midi / Bonsoir).
- ** Notifications toast** non bloquantes pour les actions réussies (à la place
  des pop-ups modales), avec animation d'entrée/sortie.
- ** Analyses intelligentes** : recommandations générées automatiquement
  (niveau d'occupation, réservations en attente, type le plus rentable, durée
  moyenne de séjour, tarif moyen).
- ** Compteurs animés** : les indicateurs du tableau de bord s'animent de 0
  jusqu'à leur valeur.

---

##  Fonctionnalités

- **Gestion des chambres** (entité 1) : ajout, modification, suppression,
  recherche instantanée, filtrage par type, export CSV.
- **Gestion des réservations** (entité 2) : liées à une chambre par clé
  étrangère, avec **calcul automatique du montant** (nombre de nuits × prix),
  validation des saisies (dates, capacité, email) et filtrage par statut.
- **Tableau de bord** : indicateurs clés (chambres, disponibilités, chiffre
  d'affaires), **taux d'occupation** (ProgressBar), **graphiques** (PieChart
  des types, BarChart des statuts), activité récente et **personnalisation du
  thème** en direct (ColorPicker).
- **Synchronisation automatique** des données entre les onglets.
- Interface soignée avec **thème CSS dynamique**.

---

##  Architecture du projet

```
HotelManagerFX/
├── pom.xml                      # Configuration Maven (dépendances + exécution)
├── init_db.sql                 # Script de création de la base + données d'exemple
├── README.md
└── src/main/
    ├── java/ma/ensao/hotel/
    │   ├── MainApp.java                 # Point d'entrée JavaFX
    │   ├── model/                       # Modèle (POJO + énumérations)
    │   │   ├── Chambre.java
    │   │   ├── Reservation.java
    │   │   ├── TypeChambre.java
    │   │   └── StatutReservation.java
    │   ├── dao/                         # Accès aux données (JDBC)
    │   │   ├── Database.java            # Connexion MySQL (Singleton)
    │   │   ├── DAO.java                 # Interface CRUD générique
    │   │   ├── ChambreDAO.java
    │   │   └── ReservationDAO.java
    │   ├── controller/                  # Contrôleurs (logique des vues)
    │   │   ├── MainController.java
    │   │   ├── ChambreController.java
    │   │   ├── ReservationController.java
    │   │   └── DashboardController.java
    │   └── util/                        # Utilitaires transverses
    │       ├── AlertUtil.java           # Boîtes de dialogue
    │       ├── CSVExporter.java         # Export CSV
    │       └── ThemeManager.java        # Thème dynamique
    └── resources/
        ├── database.properties          # Identifiants de connexion
        └── ma/ensao/hotel/
            ├── view/                    # Interfaces FXML
            │   ├── MainView.fxml
            │   ├── ChambreView.fxml
            │   ├── ReservationView.fxml
            │   └── DashboardView.fxml
            └── css/styles.css           # Feuille de style
```

**Rôle de chaque couche :**

| Couche | Responsabilité |
|--------|----------------|
| `model` | Représente les données métier (chambres, réservations) et les types énumérés. |
| `dao` | Dialogue avec MySQL : requêtes SQL, mapping `ResultSet` ↔ objets. |
| `controller` | Relie les vues FXML à la logique : événements, validation, mise à jour. |
| `view` (FXML) | Décrit l'interface graphique de façon déclarative. |
| `util` | Services réutilisables (alertes, export, thème). |

---

##  Prérequis

- **JDK 17** ou supérieur
- **Apache Maven 3.8+**
- **MySQL Server 8.x** (avec un compte ayant les droits de création de base)

> JavaFX et le pilote MySQL sont récupérés automatiquement par Maven : aucune
> installation manuelle de JavaFX n'est nécessaire.

---

##  Installation et lancement

### 1. Créer la base de données

```bash
mysql -u root -p < init_db.sql
```

Le script crée la base `hotel_db`, les tables `chambres` et `reservations`,
les index, puis insère un jeu de données d'exemple.

### 2. Configurer la connexion

Modifiez `src/main/resources/database.properties` selon votre installation :

```properties
db.url=jdbc:mysql://localhost:3306/hotel_db?useSSL=false&serverTimezone=UTC&allowPublicKeyRetrieval=true&characterEncoding=UTF-8
db.user=root
db.password=VOTRE_MOT_DE_PASSE
```

### 3. Lancer l'application

```bash
mvn clean javafx:run
```

---

##  Composants JavaFX utilisés

L'application met en œuvre l'ensemble des contrôles attendus :

| Catégorie | Contrôles |
|-----------|-----------|
| Saisie texte | `TextField`, `TextArea` |
| Sélection | `ComboBox`, `RadioButton` + `ToggleGroup`, `CheckBox` |
| Numérique / date | `Spinner`, `Slider`, `DatePicker` |
| Affichage données | `TableView`, `ListView`, `Label` |
| Progression | `ProgressBar`, `ProgressIndicator` |
| Graphiques | `PieChart`, `BarChart` |
| Conteneurs | `TabPane`, `Accordion` / `TitledPane`, `BorderPane`, `GridPane` |
| Navigation | `MenuBar` / `Menu` / `MenuItem` |
| Dialogues | `Alert`, `Dialog` |
| Personnalisation | `ColorPicker`, `Tooltip`, `FileChooser` |

---

##  Auteurs

Projet réalisé par :
Zouhir Attanouti - Mazen El Allali

