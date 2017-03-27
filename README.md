🍃 Flyweight
---------
**Les problèmes à résoudre**
> La création d'objet prend du temps. On est d'accord, à chaque nouvel objet il faut exécuter tout le code correspondant. Le problème c'est que **l'exécution de ce code peut prendre du temps**. On ne parle peut-être pas de minutes, mais même quelques millisecondes on déjà un impacte.

> Un autre problème est celui du stockage. **Chaque objet occupe de l'espace en mémoire**. Il faut donc faire attention à bien gérer l'espace mémoire occupé par nos objets, histoire de ne pas en être à court un jour.

**Intentions du pattern**
> Réduire l'espace mémoire utilisé par les objets

> Réduire les dépenses en termes de calculs

> Gagner du temps d'éxécution pour notre application

**Comment ?**
> Partager, autant que possible, des données similaires entre plusieurs objets.

> **Données intrinsèques**: dépendent de l'objet en question

> **Données extrinsèques**: ne dépendent pas de l'objet en question

> On va donc faire en sorte que les objets partagent entre eux des données extrinsèques.

Exemple: **Les Strings**
> C'est typiquement ce qu'il arrive avec les objets String. Je pense que nous sommes d'accord sur le fait que deux variables contenant la même String (même suite de caractère) pointent sur le même objet String.
```java
public class PoidsMoucheString {
    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) {
        
        String s1 = "test";
        String s2 = "foo";

       // Affichage
        System.out.println(s1 + " hashcode : " + s1.hashCode() );
        System.out.println(s2 + " hashcode : " + s2.hashCode() );

       System.out.println("##################");

       // On change ici la valeur de la chaîne
        s2 = "test"; // on assigne "test" à la variable s2
        System.out.println(s1 + " hashcode : " + s1.hashCode() );
        System.out.println(s2 + " hashcode : " + s2.hashCode() ); // On se rend compte que les 2 variables pointent sur le même objet 
    }   
}
```

Exemple: **Les éditeurs de texte**
> Typiquement pour un éditeur de texte on implémente ce design pattern. Imaginons un texte de 100'000 caractères (ça va vite), on ne va pas créer de A à Z 25'093 lettres "e". Ces 25'093 utilisent toutes la même police, voici typiquement une donnée extrinsèque (s'applique à tous les objets "e" en question).

**Quand utiliser ce design pattern ?**
> Lorsque nous avons beaucoup d'objets dans notre application.

> Lorsque les coûts en stockage sont élevés.

> Les objets possèdent des mêmes caractéristiques. (ils pourront donc se partager des données)

> L'application n'a pas besoin d'objet unique.

> **‼️ Ne pas utiliser si l'information partagée peut varier à travers le temps ‼️** 


Exemple: **Créations d'arbres**
> Admettons que dans notre application nous voulons créer des arbres identiques. Les seules choses qui les différencient sont leurs positions X et Y.
> Nos données extrinsèques seront les données relatives à l'aspect visuels des arbres.
> Nos données intrinsèques seront les données de positions. Ces donnée sont propres à chaques arbres.
```java
import java.util.Arrays;
public class PoidsMoucheSimple {
    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) throws InterruptedException {

        Arbre monArbre = new Arbre(10, 32);
        Arbre monDeuxiemeArbre = new Arbre(0, 2);
        
        // Regardons si les deux arbres on bien les mêmes feuilles
        System.out.println(monArbre.lesFeuilles); // check l'adresse du tableau
        System.out.println(Arrays.toString(monArbre.lesFeuilles)); // check l'adresse de chaque feuille
        monArbre.getCoordonnees();
        System.out.println(monDeuxiemeArbre.lesFeuilles); // check l'adresse du tableau 
        System.out.println(Arrays.toString(monDeuxiemeArbre.lesFeuilles)); // check l'adresse de chaque feuille
        monDeuxiemeArbre.getCoordonnees();
        
        // Dorénavant, n'importe quel arbre qui sera crée pointera sur le même
        // tableau tout en ayant ses propres propriétés x et y.*/
   }
}
```

```java
class Feuilles {
    private final int feuilles;
    
    public Feuilles() throws InterruptedException{
        this.feuilles = 1;
        System.out.println("Je genère une feuille");    
        Thread.sleep(1000); // simule le fait que ça prend du temps
    }
}
```
```java 
class Arbre {
    private static Feuilles[] feuilles;
    public Feuilles[] lesFeuilles; // En public juste pour tester l'adresse dans le main
    private Feuilles[] desFeuilles; // intérmédiaire, utilisé une fois pour la méthode getFeuilles(
    private int x;
    private int y;
    
    public Arbre(int leX, int leY) throws InterruptedException{
        // Il faut créer les Feuilles ici
        if (feuilles != null){
            // les Feuilles ont déjà étée crée
            this.lesFeuilles = feuilles;
        } else {
            // il faut créer les Feuilles (arrivera que lors de la création du premier arbre)
            feuilles = getFeuilles();
            this.lesFeuilles = feuilles;
        } 
        this.x = leX;
        this.y = leY;
        System.out.println("Un arbre apparaît.");
    }
    
    public Feuilles[] getFeuilles() throws InterruptedException{
        desFeuilles = new Feuilles[10];
        for(int i = 0 ; i < 10 ; i ++){
            desFeuilles[i] = new Feuilles();
        }
        return desFeuilles;
    }
    
    public void getCoordonnees(){
        System.out.println("En x: " + this.x + " | En y: " + this.y);
    }
}
```

Exemple: **Créations d'arbres, version Avancée**

> Ici on utilise aussi la Factory
```java
import java.util.Arrays;
/**
 * FLYWEIGHT - POIDS MOUCHE
 * DESIGN PATTERN
 * @author choubidouap
 */
public class PoidsMoucheAvance {
    /**
     * @param args the command line arguments
     * @throws java.lang.InterruptedException
     */
    public static void main(String[] args) throws InterruptedException {
        Arbre[] arbres;
        int nombreArbres;
        
        // Combien d'arbres seront génerés
        nombreArbres = 100000;
        // set la taille du tableau
        arbres = new Arbre[nombreArbres];
        // Générer le nombre d'arbre choisis
        for (int i = 0 ; i < nombreArbres ; i ++){
            arbres[i] = ArbreFactory.getArbre(getRandomTaille()); // ici on cale un random
        }
        
        System.out.println("Nombres d'arbres: " + arbres.length);
        System.out.println("Nombres d'objets arbre: " + ArbreFactory.nombreObjets());
        
/*--------- TESTS POUR CONSTATER LE FONCTIONNEMENT DU FLYWEIGHT ---------*/
       // test les feuilles
        System.out.println(Arrays.toString(arbres[5].lesFeuilles));
        System.out.println(Arrays.toString(arbres[120].lesFeuilles));
        System.out.println(Arrays.toString(arbres[32].lesFeuilles));
        System.out.println(Arrays.toString(arbres[82].lesFeuilles));
        // test les arbres
        System.out.println(Arrays.toString(arbres));
    }
    
    // Taille minimum et maximum des arbres
    static int tailleMin = 20; // compris
    static int tailleMax = 27; // non-compris
    /**
     * Génère un chiffre aléatoire compris entre un maximum et minimum spécifié
     * @return le chiffre généré 
     */
    private static int getRandomTaille() {
       return (int)(Math.random() * (tailleMax-tailleMin)) + tailleMin;
    }
}
```

```java
class Feuilles {
    private final int feuilles;
    
    public Feuilles() throws InterruptedException{
        this.feuilles = 1; // Juste du remplissage
        System.out.println("Je genère une feuille");
        Thread.sleep(500); // Simuler un temps d'éxectution long (en millisecondes)
    }
}
```

```java
public class ArbreFactory {
    // La clé sera la taille de l'arbre
    private static final HashMap<Integer, Arbre> arbresMap = new HashMap();
    
    /**
     * Va transmettre un arbre de la taille demandée. Le créera s'il n'existe pas encore. 
     * @param taille
     * @return un arbre de la taille correspondante
     * @throws InterruptedException 
     */
    public static Arbre getArbre(int taille) throws InterruptedException{
        if(arbresMap.containsKey(taille)){ // Si un arbre de la taille correspondante existe
            System.out.println("Cet arbre existe.");
            return arbresMap.get(taille); // on retourne l'arbre correspondant
        } else {
            // Cet arbre n'existe pas, alors on le crée et on le met dans le HashMap            
            System.out.println("Un arbre est créé.");
            arbresMap.put(taille, new Arbre(taille)); // création de l'arbre et ajout dans le HashMap
            return arbresMap.get(taille); // on retourne l'arbre correspondant
        }
    }
    
    public static int nombreObjets(){
        return arbresMap.size();
    }
}
```

```java
class Arbre {
    private static Feuilles[] feuilles;
    public Feuilles[] lesFeuilles; // En public juste pour tester l'adresse dans le main
    private int taille;
    
    /**
     * Crée un arbre de la taille définie en argument
     * @param laTaille
     * @throws InterruptedException 
     */
    public Arbre(int laTaille) throws InterruptedException{
        // Test pour savoir si des feuilles ont déjà été créée
        if (feuilles != null){
            // les Feuilles ont déjà étée créée
            this.lesFeuilles = feuilles;
        } else {
            // il faut créer les Feuilles (arrivera que lors de la création du premier arbre)
            feuilles = getFeuilles();
            this.lesFeuilles = feuilles;
        } 
        this.taille = laTaille;
        Thread.sleep(2000);
    }
    
    /**
     * Va créer des feuilles
     * @return
     * @throws InterruptedException 
     */
    public Feuilles[] getFeuilles() throws InterruptedException{
        Feuilles[] desFeuilles;
        int nombreFeuilles = 10;
        
        desFeuilles = new Feuilles[nombreFeuilles];
        for(int i = 0 ; i < nombreFeuilles ; i ++){
            desFeuilles[i] = new Feuilles();
        }
        return desFeuilles;
    }
    
    public void getCoordonnees(){
        System.out.println("Taille de l'arbre: " + this.taille);
    }
}
```
