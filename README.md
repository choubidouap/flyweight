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
**Programmatic example**

Translating our tea example from above. First of all we have tea types and tea maker

```java
class TeaMaker
{
    protected $availableTea = [];

    public function make($preference)
    {
        if (empty($this->availableTea[$preference])) {
            $this->availableTea[$preference] = new KarakTea();
        }

        return $this->availableTea[$preference];
    }
}
```
