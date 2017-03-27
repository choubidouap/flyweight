🍃 Flyweight
---------
Les problèmes à résoudre
> La création d'objet prend du temps. On est d'accord, à chaque nouvel objet il faut exécuter tout le code correspondant. Le problème c'est que **l'exécution de ce code peut prendre du temps**. On ne parle peut-être pas de minutes, mais même quelques millisecondes on déjà un impacte.

> Un autre problème est celui du stockage. **Chaque objet occupe de l'espace en mémoire**. Il faut donc faire attention à bien gérer l'espace mémoire occupé par nos objets, histoire de ne pas en être à court un jour.

Intentions du pattern
> Réduire l'espace mémoire utilisé par les objets

> Réduire les dépenses en termes de calculs

> Gagner du temps d'éxécution pour notre application

Comment ?
> Partager, autant que possible, des données similaires entre plusieurs objets.

   > **Données intrinsèques**: dépendent de l'objet en question

   > **Données extrinsèques**: ne dépendent pas de l'objet en question

> On va donc faire en sorte que les objets partagent entre eux des données extrinsèques.

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
