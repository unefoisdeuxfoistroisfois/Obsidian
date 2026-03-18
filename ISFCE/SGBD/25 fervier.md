1. URL deja fais avant
2. Info de connexion
	1. ou elle est situré 
3. Création de la connexion
	1. grace a un singletion (une classe qui va géré sa).

crée un interfac equi aura une seule methode get propertie

Avoir un objet properite qui contiendra une clé valeur. Qui va etre chargé dans un dictionnaire. Le prof a mis dans ConnexionFB_empolyee.java, qui se trouve dans src/database/connexion/. dans ce fichier on verra un @overrides qui implementera deaj une methode existante qui est celle de dans le fichier IconnexionInfos.java.

Il faudra utlisé des clé reconnue pas gdbc. Sa se trouve de Test

on a un fichier "connexion.properties" qui provien de pdb_example qui provient du github du prof a fin de nous servir d'exemple. il est dans ressources je vais juste le copier et coller dans dossier ressource a moi.

Commetn on fait pour se connecté a une base de données ? 
```java

public class TestConnect {
    public static void main(String[] args){
        try {
            IConnexionInfos info = new ConnexionFromFile("/ressources/connexion.properties", Databases.FIREBIRD);
            // Objet de conenxion singletion
            ConnexionSingleton.setInfoConnexion(info);
            Connection connect = ConnexionSingleton.getConnexion();
			
            log.info("okok");
            ConnexionSingleton.liberationConnexion();
        } catch (Exception e) {
            log.error(e.getMessage());
        }
    }
}
```

etape suivante maintantt que on a une connextion ? voir commetn on va structurer un projet impliqué le disigne pattern DAO c'est la que le sql va etre en jeu que pour la partie DAO.