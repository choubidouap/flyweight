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

>> **Données intrinsèques**: dépendent de l'objet en question

>> **Données extrinsèques**: ne dépendent pas de l'objet en question

> On va donc faire en sorte que les objets partagent entre eux des données extrinsèques.

**Les Strings**
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
        s2 = "test";
        System.out.println(s1 + " hashcode : " + s1.hashCode() );
        System.out.println(s2 + " hashcode : " + s2.hashCode() );       
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
