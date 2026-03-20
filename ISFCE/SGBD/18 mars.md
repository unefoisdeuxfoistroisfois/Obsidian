Le prof a fait un GitLab, on git clone son projet pour avoir une base.

Création d'un fichier pour la connexion à la base de données à ajouter dans le dossier ressources, on l'appelle `connexionPDB2526.properties`. Télécharger aussi la base de données qui est sur Moodle.

```properties
file : #le chemin de la base de données PDB2526.FDB
autoCommit : false
user : SYSDBA
password : masterkey
encoding : UTF8
```

Ensuite un copier-coller du fichier juste pour avoir une copie `connexionPDB2526_test.properties` :

```properties
file : #le chemin de la base de données PDB2526_TEST.FDB
autoCommit : false
user : SYSDBA
password : masterkey
encoding : UTF8
```

Ne pas oublier de regarder la vidéo pour installer JavaFX.

## JavaFX sur VS Code

### .classpath (vient d'Eclipse)
- Le `.classpath` est généré par Eclipse (fourni par le prof)
- Il définit les chemins vers les JARs du projet
- **VS Code lit le `.classpath` en priorité** avant le `java.project.referencedLibraries` du `settings.json`
- Si on veut que VS Code utilise le `settings.json` à la place, il faut supprimer le `.classpath`
- J'ai dû adapter le `.classpath` du prof pour y ajouter le chemin vers le SDK JavaFX local

### e(fx)clipse (Eclipse) = launch.json (VS Code)
- e(fx)clipse est un plugin Eclipse qui configure automatiquement `--module-path` et `--add-modules` pour JavaFX et génère les configs de lancement en quelques clics
- Dans VS Code on fait la même chose manuellement via le `launch.json` -> pas besoin d'e(fx)clipse

### launch.json (spécifique à VS Code) — uniquement pour JavaFX
- Eclipse stocke sa config de lancement dans un `.launch` (dans le workspace Eclipse, pas dans le projet)
- VS Code a besoin d'un `.vscode/launch.json` dans le projet
- Le `launch.json` passe des VM args à la JVM pour lui indiquer où trouver les modules JavaFX :
  - `--module-path` -> chemin vers les JARs du SDK JavaFX
  - `--add-modules` -> modules JavaFX à charger (javafx.controls, javafx.fxml, javafx.graphics)

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "java",
            "name": "Launch Pdb2526",
            "request": "launch",
            "mainClass": "org.isfce.pdb.Pdb2526",
            "projectName": "pdb2526",
            "vmArgs": "--module-path /Users/bradley/javafx-sdk-25.0.2/lib --add-modules javafx.controls,javafx.fxml,javafx.graphics"
        }
    ]
}
```

En gros, JavaFX a besoin d'arguments, il faut lui en donner — notre `launch.json` le fait pour nous.

## .project (Eclipse / VS Code Java)

- Fichier de configuration Eclipse, lu aussi par le Java Language Server de VS Code
- Décrit le projet à l'IDE (nom, type, builders)
- **Sans ce fichier correctement configuré, VS Code ne reconnaît pas le projet comme Java -> tout est rouge**

### Structure clé
- `<name>` -> nom du projet dans l'IDE
- `<buildSpec>` -> définit `org.eclipse.jdt.core.javabuilder` (compile le Java)
- `<natures>` -> définit `org.eclipse.jdt.core.javanature` (indique que c'est un projet Java)
- `<filteredResources>` -> dossiers/fichiers ignorés (`node_modules`, `.git`, etc.)

### Ce qu'on a dû corriger
Le `.project` fourni par le prof n'avait pas le builder ni la nature Java (balises vides).
On les a ajoutés manuellement -> VS Code a ensuite reconnu le projet correctement.

> Si VS Code reste en rouge après modification du `.project` :
> `Ctrl+Shift+P` -> **"Java: Clean Java Language Server Workspace"**

## Fichiers de configuration Eclipse / VS Code Java

### `.project`
- Carte d'identité du projet pour l'IDE
- Contient le nom du projet, le type (nature) et le builder
- Sans `javanature` + `javabuilder` -> l'IDE ne reconnaît pas le projet comme Java -> tout est rouge

```xml
<buildSpec>
    <buildCommand>
        <name>org.eclipse.jdt.core.javabuilder</name>
        <arguments>
        </arguments>
    </buildCommand>
</buildSpec>

<nature>org.eclipse.jdt.core.javanature</nature>
```

### `.classpath`
- Sac à dos du projet pour l'IDE
- Liste toutes les dépendances : JARs, dossiers source (`src/`), dossiers de sortie (`bin/`)
- VS Code lit le `.classpath` en priorité avant le `java.project.referencedLibraries` du `settings.json`

### Git
- `.project` -> **à commiter** (tout le monde en a besoin)
- `.classpath` -> **à commiter** (tout le monde en a besoin)
- `.vscode/` -> **à ignorer** (spécifique VS Code, contient des chemins locaux)
