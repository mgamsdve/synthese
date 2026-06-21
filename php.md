# Fiche de révision — PHP / SQL / JavaScript — CollabSphere 2026

Cette fiche résume **ce que tu as dû créer pendant les révisions** et les patterns à savoir refaire rapidement à l’examen : routes PHP, formulaires, modèles SQL, assignations en base, suppressions, conditions de droits, affichage dynamique et filtres JavaScript.

---

## 1. Logique générale de la codebase

La codebase fonctionne comme un petit MVC maison :

```text
index.php
 ├── démarre la session
 ├── charge la connexion BDD
 ├── charge les controllers
 └── charge Views/base.php si $template existe
```

Dans chaque controller, on utilise :

```php
$uri = $_SERVER["REQUEST_URI"];
```

Puis on compare l’URL avec des `if / elseif` :

```php
if ($uri == "/creerRole") {
    // traitement de la page
    $title = "Créer un rôle";
    $template = "Views/Roles/creerRole.php";
}
```

Le controller prépare les données, choisit un titre et choisit une vue :

```php
$title = "Titre de la page";
$template = "Views/Dossier/page.php";
```

Ensuite `base.php` affiche automatiquement la vue :

```php
<?php require_once($template) ?>
```

À retenir : **si tu crées une nouvelle page, tu dois généralement modifier 3 fichiers** :

```text
1. Controller : ajouter la route
2. Model : ajouter les fonctions SQL
3. View : créer le formulaire ou l’affichage
```

---

## 2. Routes PHP à connaître

### Route simple sans paramètre GET

Exemple : page de création des rôles.

```php
if ($uri == "/creerRole") {
    $roles = selectAllRoles($pdo);

    if (isset($_POST["ajouter"])) {
        insertRole($pdo);
        $message = "Votre rôle a bien été créé";
    }

    if (isset($_POST["supprimer"])) {
        deleteRole($pdo);
        $message = "Votre rôle a bien été supprimé";
    }

    $title = "Créer un rôle";
    $template = "Views/Roles/creerRole.php";
}
```

À utiliser quand l’URL ressemble à :

```text
/creerRole
/connexion
/inscription
```

---

### Route avec paramètre GET

Exemple : assigner un rôle dans un projet précis.

```php
} elseif (isset($_GET["projetId"]) && $uri == "/assignerRole?projetId=" . $_GET["projetId"]) {
    if (isset($_POST["envoyer"])) {
        assignRoleUsersProject($pdo);
        header("location:/voirHistorique?projetId=" . $_GET["projetId"]);
    }

    $roles = selectAllRoles($pdo);
    $members = selectUsersFromProject($pdo, "member", $_GET["projetId"]);

    $title = "Assigner un rôle";
    $template = "Views/Roles/assignerRole.php";
}
```

À utiliser quand l’URL ressemble à :

```text
/voirProjet?projetId=1
/voirHistorique?projetId=1
/assignerRole?projetId=1
/modifierProjet?projetId=1
```

Point important : dans cette codebase, l’URL est comparée exactement. Donc il faut écrire la route dans le même format que le lien.

---

## 3. Boutons et liens conditionnels en PHP

### Bouton visible seulement si l’utilisateur est connecté

```php
<?php if (isset($_SESSION["user"])) : ?>
    <h3 class="creer"><a href="creerRole">Créer un rôle</a></h3>
<?php endif ?>
```

À utiliser pour :

```text
- créer un projet
- créer un rôle
- devenir membre
- supprimer son compte
- toute action réservée aux utilisateurs connectés
```

---

### Bouton visible seulement pour le créateur du projet

```php
<?php if(isset($_SESSION["user"]) && $projet->owner_id == $_SESSION["user"]->users_id) : ?>
    <h3 class="creer">
        <a href="assignerRole?projetId=<?= $projet->id ?>">Assigner des rôles</a>
    </h3>
<?php endif ?>
```

À utiliser pour :

```text
- modifier un projet
- supprimer un projet
- créer une tâche
- assigner des rôles
- toute action réservée au propriétaire
```

Le test important est :

```php
$projet->owner_id == $_SESSION["user"]->users_id
```

---

## 4. Formulaires PHP importants

### Formulaire simple d’ajout

Exemple : créer un rôle.

```php
<form action="" method="post">
    <div>
        <label for="role">Nom du rôle</label>
        <input type="text" name="role" id="role" required>
    </div>

    <div>
        <button type="submit" name="ajouter">Ajouter</button>
    </div>
</form>
```

Dans le controller :

```php
if (isset($_POST["ajouter"])) {
    insertRole($pdo);
    $message = "Votre rôle a bien été créé";
}
```

À retenir : le controller repère le formulaire grâce au `name` du bouton :

```php
name="ajouter"
```

---

### Formulaire de suppression avec un select

Exemple : supprimer un rôle.

```php
<form action="" method="post">
    <div>
        <label for="role">Choisis un rôle</label>
        <select name="roleId" id="roles" required>
            <?php foreach ($roles as $role) : ?>
                <option value="<?= $role->role_id ?>">
                    <?= $role->role_nom ?>
                </option>
            <?php endforeach ?>
        </select>
    </div>

    <div>
        <button type="submit" name="supprimer">Supprimer</button>
    </div>
</form>
```

Dans le controller :

```php
if (isset($_POST["supprimer"])) {
    deleteRole($pdo);
    $message = "Votre rôle a bien été supprimé";
}
```

Dans le model :

```php
function deleteRole($pdo){
    try {
        $query = "DELETE FROM roles where role_id=:id;";
        $delete = $pdo->prepare($query);
        $delete->execute([
            "id" => $_POST["roleId"]
        ]);
    } catch (PDOException $e) {
        $message = $e->getMessage();
        die($message);
    }
}
```

---

### Formulaire avec select multiple

Exemple : assigner un rôle à plusieurs membres.

```php
<form action="" method="post">
    <div>
        <label for="role">Choisis un rôle</label>
        <select name="roleId" id="roles" required>
            <?php foreach ($roles as $role) : ?>
                <option value="<?= $role->role_id ?>">
                    <?= $role->role_nom ?>
                </option>
            <?php endforeach ?>
        </select>
    </div>

    <div>
        <label for="members">Choisissez les utilisateurs</label>
        <select name="members[]" id="members" multiple>
            <?php foreach ($members as $member) : ?>
                <option value="<?= $member->users_id ?>">
                    <?= $member->username ?>
                </option>
            <?php endforeach ?>
        </select>
    </div>

    <div>
        <button type="submit" name="envoyer">Envoyer</button>
    </div>
</form>
```

Point crucial :

```php
name="members[]"
```

Le `[]` est obligatoire pour recevoir plusieurs utilisateurs dans :

```php
$_POST["members"]
```

---

## 5. Modèles SQL à connaître

Tous les modèles suivent ce modèle :

```php
function maFonction($pdo){
    try {
        $query = "REQUETE SQL";
        $statement = $pdo->prepare($query);
        $statement->execute([
            "param" => $valeur
        ]);
        return $statement->fetchAll();
    } catch (PDOException $e) {
        $message = $e->getMessage();
        die($message);
    }
}
```

---

### INSERT simple

Exemple : créer un rôle.

```php
function insertRole($pdo){
    try {
        $query = "insert into roles(role_nom) VALUES (:role_nom)";
        $insert = $pdo->prepare($query);
        $insert->execute([
            "role_nom" => $_POST["role"],
        ]);
    } catch (PDOException $e) {
        $message = $e->getMessage();
        die($message);
    }
}
```

À adapter pour créer :

```text
- une catégorie
- un statut
- une priorité
- un tag
- un rôle
```

---

### SELECT simple

Exemple : récupérer tous les rôles.

```php
function selectAllRoles($pdo){
    try {
        $query = "SELECT * FROM roles";
        $select = $pdo->prepare($query);
        $select->execute();
        $roles = $select->fetchAll();
        return $roles;
    } catch (PDOException $e) {
        $message = $e->getMessage();
        die($message);
    }
}
```

---

### SELECT avec WHERE

Exemple : récupérer un projet par son id.

```php
function selectOneProjectById($pdo, $projetId){
    try {
        $query = "SELECT * FROM projects where id = :id";
        $select = $pdo->prepare($query);
        $select->execute([
            "id" => $projetId
        ]);
        $projet = $select->fetch();
        return $projet;
    } catch (PDOException $e) {
        $message = $e->getMessage();
        die($message);
    }
}
```

À utiliser dès que tu dois récupérer **un élément précis** avec un id en GET.

---

### SELECT avec JOIN

Exemple : récupérer les projets avec le créateur.

```php
function selectAllProject($pdo){
    try {
        $query = "SELECT * FROM projects join users on projects.owner_id = users.users_id where is_public = 1;";
        $select = $pdo->prepare($query);
        $select->execute();
        $projets = $select->fetchAll();
        return $projets;
    } catch (PDOException $e) {
        $message = $e->getMessage();
        die($message);
    }
}
```

À retenir : un `JOIN` sert quand tu veux récupérer des infos de plusieurs tables.

Structure générale :

```sql
SELECT *
FROM table1
JOIN table2 ON table1.champ_id = table2.champ_id
WHERE condition = 1;
```

---

### SELECT avec sous-requête

Exemple : récupérer les utilisateurs membres d’un projet.

```php
function selectUsersFromProject($pdo, $role, $projetId){
    try {
        $query = "select * from users where users_id in (select user_id from project_users where project_id = :project_id and role=:role);";
        $select = $pdo->prepare($query);
        $select->execute([
            "project_id" => $projetId,
            "role" => $role
        ]);
        $users = $select->fetchAll();
        return $users;
    } catch (PDOException $e) {
        $message = $e->getMessage();
        die($message);
    }
}
```

À comprendre :

```sql
SELECT * FROM users
WHERE users_id IN (
    SELECT user_id FROM project_users
    WHERE project_id = :project_id
);
```

Ça veut dire : “récupère les utilisateurs dont l’id est présent dans une table de liaison”.

---

### INSERT dans une table de liaison

Exemple : devenir membre d’un projet.

```php
function becomeMember($pdo, $projetId){
    try {
        $query = "insert into project_users(project_id, user_id, role) VALUES (:project_id, :user_id, :role)";
        $insert = $pdo->prepare($query);
        $insert->execute([
            "project_id" => $projetId,
            "user_id" => $_SESSION["user"]->users_id,
            "role" => "member"
        ]);
    } catch (PDOException $e) {
        $message = $e->getMessage();
        die($message);
    }
}
```

À retenir : pour une table de liaison, on insère souvent plusieurs ids :

```text
project_id
user_id
role_id
```

---

### INSERT multiple avec foreach

Exemple : assigner le même rôle à plusieurs membres.

```php
function assignRoleUsersProject($pdo){
    try {
        $query = "insert into role_users_projet(user_id, role_id, projet_id) VALUES (:userId, :roleId, :projetId)";
        $insert = $pdo->prepare($query);

        foreach ($_POST["members"] as $userId) {
            $insert->execute([
                "userId" => $userId,
                "roleId" => $_POST["roleId"],
                "projetId" => $_GET["projetId"]
            ]);
        }
    } catch (PDOException $e) {
        $message = $e->getMessage();
        die($message);
    }
}
```

C’est un des patterns les plus importants des révisions.

À retenir :

```php
foreach ($_POST["members"] as $userId) {
    // une insertion par utilisateur sélectionné
}
```

---

### SELECT avec LEFT JOIN pour afficher les rôles d’un membre

Exemple : récupérer les rôles d’un utilisateur dans un projet.

```php
function selectRoleUserProjectByUserId($pdo, $userId, $projetId)
{
    try {
        $query = "
            SELECT *
            FROM role_users_projet
            LEFT JOIN roles
                ON role_users_projet.role_id = roles.role_id
            WHERE role_users_projet.user_id = :userId
              AND role_users_projet.projet_id = :projetId;
        ";

        $select = $pdo->prepare($query);
        $select->execute([
            "userId" => $userId,
            "projetId" => $projetId
        ]);

        $roles = $select->fetchAll();
        return $roles;
    } catch (PDOException $e) {
        $message = $e->getMessage();
        die($message);
    }
}
```

À comprendre :

```text
role_users_projet contient les ids
roles contient les noms des rôles
```

Donc il faut joindre les deux tables pour afficher `role_nom`.

---

### DELETE simple

Exemple : supprimer un rôle.

```php
function deleteRole($pdo){
    try {
        $query = "DELETE FROM roles where role_id=:id;";
        $delete = $pdo->prepare($query);
        $delete->execute([
            "id" => $_POST["roleId"]
        ]);
    } catch (PDOException $e) {
        $message = $e->getMessage();
        die($message);
    }
}
```

---

### DELETE depuis une URL GET

Exemple : supprimer une assignation rôle/utilisateur/projet avec une croix.

```php
function deleteRoleUser($pdo){
    try {
        $query = "DELETE FROM role_users_projet where role_users_projet_id=:id;";
        $delete = $pdo->prepare($query);
        $delete->execute([
            "id" => $_GET["role_user_project_id"]
        ]);
    } catch (PDOException $e) {
        $message = $e->getMessage();
        die($message);
    }
}
```

Route correspondante dans le controller :

```php
} elseif (isset($_GET["role_user_project_id"])) {
    deleteRoleUser($pdo);
    header("location:/voirHistorique?projetId=" . $_GET["projetId"]);
}
```

Lien possible dans la vue :

```php
<a href="supprimerRole?projetId=<?= $_GET["projetId"] ?>&role_user_project_id=<?= $role->role_users_projet_id ?>">x</a>
```

Dans cette codebase, le nom exact de la route importe moins que la présence du paramètre :

```php
$_GET["role_user_project_id"]
```

---

### DELETE de l’utilisateur connecté

Exemple : supprimer son compte.

```php
function deleteUser($pdo){
    try {
        $query = "DELETE from users where users_id = :userId";
        $delete = $pdo->prepare($query);
        $delete->execute([
            "userId" => $_SESSION["user"]->users_id
        ]);
    } catch (PDOException $e) {
        $message = $e->getMessage();
        die($message);
    }
}
```

Dans le controller :

```php
if(isset($_POST["supprimer"])){
    deleteUser($pdo);
    header("location:/deconnexion");
}
```

---

## 6. Afficher les données dans une vue

### Afficher une liste simple

Exemple : liste des membres.

```php
<?php foreach($members as $member) : ?>
    <p><?= $member->username ?></p>
<?php endforeach ?>
```

---

### Afficher les rôles à côté de chaque membre

Pattern probable à connaître pour l’examen :

```php
<?php foreach($members as $member) : ?>
    <p>
        <?= $member->username ?>

        <?php
            $rolesUser = selectRoleUserProjectByUserId($pdo, $member->users_id, $_GET["projetId"]);
        ?>

        <?php foreach($rolesUser as $roleUser) : ?>
            <span>
                <?= $roleUser->role_nom ?>
                <a href="supprimerRole?projetId=<?= $_GET["projetId"] ?>&role_user_project_id=<?= $roleUser->role_users_projet_id ?>">x</a>
            </span>
        <?php endforeach ?>
    </p>
<?php endforeach ?>
```

But :

```text
- afficher le nom du membre
- récupérer ses rôles pour ce projet
- afficher chaque rôle
- ajouter une croix qui supprime l’assignation
```

---

### Afficher un message

Dans le controller :

```php
$message = "Votre rôle a bien été créé";
```

Dans la vue :

```php
<?php if(isset($message)) : ?>
    <p class="message"><?= $message ?></p>
<?php endif ?>
```

Pour les erreurs :

```php
$error = "Rôle existant";
```

Dans la vue :

```php
<?php if(isset($error)) : ?>
    <p class="error"><?= $error ?></p>
<?php endif ?>
```

---

## 7. JavaScript à connaître

Le fichier `script.js` est chargé sur toutes les pages via `base.php`. Donc si tu fais du JS pour une page précise, protège ton code avec un `if`.

---

### Afficher/cacher un champ selon une checkbox

Exemple : afficher le champ GitHub si le projet est public.

HTML :

```php
<input type="checkbox" name="is_public" id="is_public" value="1">

<div id="lien_github_div" style="display:none;">
    <label for="lien_github">Lien github</label>
    <input type="url" id="lien_github" name="lien_github" placeholder="lien github">
</div>
```

JS :

### Barre de recherche JavaScript

HTML :

```php
<label for="search">Nom projet</label>
<input type="text" id="search">
```

Chaque carte doit avoir une classe commune :

```php
<div class="projetElement">
    <h2><?= $projet->title ?></h2>
    <p><?= $projet->description ?></p>
</div>
```

JS :

```js
const searchInput = document.getElementById("search");
const projectsCards = document.querySelectorAll(".projetElement");

if (searchInput) {
    searchInput.addEventListener("input", function() {
        const search = this.value.toLowerCase();

        projectsCards.forEach(card => {
            const title = card.querySelector("h2").textContent.toLowerCase();

            if (title.includes(search)) {
                card.style.display = "block";
            } else {
                card.style.display = "none";
            }
        });
    });
}
```

À retenir :

```js
textContent.toLowerCase()
includes(search)
style.display = "block"
style.display = "none"
```

---

### Filtre avec un select

HTML :

```php
<select name="creator" id="creator">
    <option value="all">Tous</option>
    <?php foreach ($users as $user) : ?>
        <option value="<?= $user->username ?>">
            <?= $user->username ?>
        </option>
    <?php endforeach ?>
</select>
```

Dans chaque carte :

```php
<p>Créé par <span class="important"><?= $projet->username ?></span></p>
```

JS :

```js
const selectCreator = document.getElementById("creator");
const projectsCards = document.querySelectorAll(".projetElement");

if (selectCreator) {
    selectCreator.addEventListener("change", function() {
        const search = this.value.toLowerCase();

        projectsCards.forEach(card => {
            const creator = card.querySelector(".important").textContent.toLowerCase();

            if (creator == search || search == "all") {
                card.style.display = "block";
            } else {
                card.style.display = "none";
            }
        });
    });
}
```

À retenir :

```js
if (creator == search || search == "all")
```

---

### Version améliorée : combiner barre de recherche + select

Le code des révisions filtre séparément. Si tu veux éviter qu’un filtre annule l’autre, tu peux faire une fonction commune :

```js
const searchInput = document.getElementById("search");
const selectCreator = document.getElementById("creator");
const projectsCards = document.querySelectorAll(".projetElement");

function filterProjects() {
    const searchTitle = searchInput ? searchInput.value.toLowerCase() : "";
    const selectedCreator = selectCreator ? selectCreator.value.toLowerCase() : "all";

    projectsCards.forEach(card => {
        const title = card.querySelector("h2").textContent.toLowerCase();
        const creator = card.querySelector(".important").textContent.toLowerCase();

        const titleOk = title.includes(searchTitle);
        const creatorOk = selectedCreator === "all" || creator === selectedCreator;

        if (titleOk && creatorOk) {
            card.style.display = "block";
        } else {
            card.style.display = "none";
        }
    });
}

if (searchInput) {
    searchInput.addEventListener("input", filterProjects);
}

if (selectCreator) {
    selectCreator.addEventListener("change", filterProjects);
}
```

C’est une meilleure version à utiliser si l’examen demande plusieurs filtres en même temps.

---

## 8. Tables et champs à retenir

### Table `users`

```text
users_id
username
email
password
```

### Table `projects`

```text
id
title
description
is_public
owner_id
lien_github
```

### Table `project_users`

```text
project_id
user_id
role
```

Sert à savoir qui est membre d’un projet.

### Table `roles`

```text
role_id
role_nom
```

Sert à stocker les rôles disponibles.

### Table `role_users_projet`

```text
role_users_projet_id
user_id
role_id
projet_id
```

Sert à assigner un rôle précis à un utilisateur précis dans un projet précis.

---

## 9. Différence importante entre les tables de liaison

### `project_users`

Cette table répond à la question :

```text
Qui est membre du projet ?
```

Exemple :

```sql
insert into project_users(project_id, user_id, role)
VALUES (:project_id, :user_id, :role)
```

---

### `role_users_projet`

Cette table répond à la question :

```text
Quel rôle a tel utilisateur dans tel projet ?
```

Exemple :

```sql
insert into role_users_projet(user_id, role_id, projet_id)
VALUES (:userId, :roleId, :projetId)
```

Piège classique : ne pas confondre ces deux tables.

---

## 10. Validation et sécurité de base

### Vérifier qu’un formulaire est envoyé

```php
if (isset($_POST["envoyer"])) {
    // traitement
}
```

Autres boutons possibles :

```php
isset($_POST["ajouter"])
isset($_POST["supprimer"])
isset($_POST["modifier"])
```

---

### Vérifier les champs obligatoires

Dans la codebase :

```php
$clesAVerifier = ["title", "description"];
$errors = verifEmptyData($clesAVerifier);

if (empty($errors)) {
    insertProject($pdo);
}
```

Pour un formulaire utilisateur :

```php
$clesAVerifier = ["username", "email", "password"];
$errors = verifEmptyData($clesAVerifier);
validateEmail("email", $errors);
```

---

### Vérifier si l’utilisateur est connecté

```php
if (isset($_SESSION["user"])) {
    // connecté
}
```

---

### Récupérer l’id de l’utilisateur connecté

```php
$_SESSION["user"]->users_id
```

---

### Redirection après traitement

```php
header("location:/voirHistorique?projetId=" . $_GET["projetId"]);
```

Autres exemples :

```php
header("location:/projetsPersonnels");
header("location:/connexion");
header("location:/deconnexion");
header("location:/");
```

---

## 11. Mini-recettes pour l’examen

### Recette 1 — Ajouter une nouvelle entité simple

Exemple : créer une catégorie, un tag, une priorité, un rôle.

1. Créer un bouton visible si connecté.
2. Créer une route dans le controller.
3. Récupérer la liste existante.
4. Créer un formulaire avec `name="ajouter"`.
5. Dans le model, écrire un `INSERT`.
6. Afficher un message.

Code controller type :

```php
if ($uri == "/creerCategorie") {
    $categories = selectAllCategories($pdo);

    if (isset($_POST["ajouter"])) {
        insertCategorie($pdo);
        $message = "Catégorie créée";
    }

    $title = "Créer une catégorie";
    $template = "Views/Categories/creerCategorie.php";
}
```

Code model type :

```php
function insertCategorie($pdo){
    try {
        $query = "insert into categories(nom) VALUES (:nom)";
        $insert = $pdo->prepare($query);
        $insert->execute([
            "nom" => $_POST["nom"]
        ]);
    } catch (PDOException $e) {
        die($e->getMessage());
    }
}
```

---

### Recette 2 — Supprimer une entité

1. Récupérer toutes les lignes avec un `SELECT`.
2. Les afficher dans un `<select>`.
3. Envoyer l’id en POST.
4. Faire un `DELETE` dans le model.
5. Afficher un message ou rediriger.

Controller :

```php
if (isset($_POST["supprimer"])) {
    deleteCategorie($pdo);
    $message = "Catégorie supprimée";
}
```

Model :

```php
function deleteCategorie($pdo){
    try {
        $query = "DELETE FROM categories WHERE categorie_id = :id";
        $delete = $pdo->prepare($query);
        $delete->execute([
            "id" => $_POST["categorieId"]
        ]);
    } catch (PDOException $e) {
        die($e->getMessage());
    }
}
```

---

### Recette 3 — Assigner une chose à plusieurs utilisateurs

Exemple : assigner un rôle, une tâche, un tag, une compétence.

Vue :

```php
<select name="elementId" required>
    <?php foreach ($elements as $element) : ?>
        <option value="<?= $element->id ?>"><?= $element->nom ?></option>
    <?php endforeach ?>
</select>

<select name="users[]" multiple>
    <?php foreach ($users as $user) : ?>
        <option value="<?= $user->users_id ?>"><?= $user->username ?></option>
    <?php endforeach ?>
</select>
```

Model :

```php
function assignElementToUsers($pdo){
    try {
        $query = "insert into table_liaison(user_id, element_id, project_id) VALUES (:userId, :elementId, :projectId)";
        $insert = $pdo->prepare($query);

        foreach ($_POST["users"] as $userId) {
            $insert->execute([
                "userId" => $userId,
                "elementId" => $_POST["elementId"],
                "projectId" => $_GET["projetId"]
            ]);
        }
    } catch (PDOException $e) {
        die($e->getMessage());
    }
}
```

---

### Recette 4 — Ajouter une croix de suppression

Dans la vue :

```php
<a href="supprimerElement?projetId=<?= $_GET["projetId"] ?>&element_user_id=<?= $element->element_user_id ?>">x</a>
```

Dans le controller :

```php
} elseif (isset($_GET["element_user_id"])) {
    deleteElementUser($pdo);
    header("location:/voirHistorique?projetId=" . $_GET["projetId"]);
}
```

Dans le model :

```php
function deleteElementUser($pdo){
    try {
        $query = "DELETE FROM table_liaison WHERE element_user_id = :id";
        $delete = $pdo->prepare($query);
        $delete->execute([
            "id" => $_GET["element_user_id"]
        ]);
    } catch (PDOException $e) {
        die($e->getMessage());
    }
}
```

---

### Recette 5 — Ajouter un filtre JavaScript

HTML :

```php
<input type="text" id="search">

<div class="elementCard">
    <h2>Nom élément</h2>
</div>
```

JS :

```js
const searchInput = document.getElementById("search");
const cards = document.querySelectorAll(".elementCard");

if (searchInput) {
    searchInput.addEventListener("input", function() {
        const search = this.value.toLowerCase();

        cards.forEach(card => {
            const title = card.querySelector("h2").textContent.toLowerCase();

            if (title.includes(search)) {
                card.style.display = "block";
            } else {
                card.style.display = "none";
            }
        });
    });
}
```

---

## 12. Checklist rapide avant de rendre

Avant de rendre, vérifier :

```text
[ ] Le bouton est au bon endroit
[ ] Le bouton est protégé par isset($_SESSION["user"]) si nécessaire
[ ] Le bouton propriétaire utilise owner_id == $_SESSION["user"]->users_id
[ ] La route existe dans le bon controller
[ ] Le $template pointe vers la bonne vue
[ ] Les données nécessaires sont récupérées avant d’afficher la vue
[ ] Le formulaire a method="post"
[ ] Les input/select ont les bons name="..."
[ ] Le bouton submit a le bon name="envoyer", "ajouter", "supprimer" ou "modifier"
[ ] Le model utilise prepare() puis execute()
[ ] Les paramètres SQL correspondent aux clés du execute()
[ ] Les redirections contiennent bien projetId si nécessaire
[ ] Les messages ou erreurs sont affichés dans la vue
[ ] Le JS vérifie que l’élément existe avant addEventListener
[ ] Les filtres JS utilisent les bonnes classes/id HTML
```

---

## 13. Erreurs classiques à éviter

### Oublier `members[]`

Mauvais :

```php
<select name="members" multiple>
```

Bon :

```php
<select name="members[]" multiple>
```

---

### Confondre POST et GET

Données de formulaire :

```php
$_POST["roleId"]
$_POST["members"]
```

Données dans l’URL :

```php
$_GET["projetId"]
$_GET["role_user_project_id"]
```

---

### Oublier de rediriger après un INSERT ou DELETE

Exemple :

```php
header("location:/voirHistorique?projetId=" . $_GET["projetId"]);
```

---

### Oublier de récupérer les données avant la vue

Mauvais :

```php
$template = "Views/Roles/assignerRole.php";
```

Bon :

```php
$roles = selectAllRoles($pdo);
$members = selectUsersFromProject($pdo, "member", $_GET["projetId"]);
$template = "Views/Roles/assignerRole.php";
```

---

### Oublier le `if` en JavaScript

Mauvais :

```js
searchInput.addEventListener("input", function() {
    // ...
});
```

Bon :

```js
if (searchInput) {
    searchInput.addEventListener("input", function() {
        // ...
    });
}
```

---

## 14. Le résumé à apprendre par cœur

Pour une fonctionnalité type examen, tu dois savoir faire ceci rapidement :

```text
1. Ajouter un lien/bouton dans une vue
2. Protéger le bouton avec une condition session/propriétaire
3. Créer une route dans le controller
4. Faire un formulaire POST
5. Récupérer les données nécessaires avec un SELECT
6. Insérer ou supprimer en base avec prepare/execute
7. Utiliser $_GET pour les ids dans l’URL
8. Utiliser $_POST pour les données du formulaire
9. Rediriger avec header("location:...")
10. Afficher les résultats avec foreach
11. Ajouter un filtre JS avec input/select + addEventListener
```

Si tu connais ces patterns, tu peux adapter presque toutes les consignes probables de l’examen.
