Juan plantea no anidar tanto como hago yo y el dice:

```sql sel
select A*, AVG(C.precio)
from Ask as A, bewer as B, control as C
 Where a.ask_id=b.ask_id AND b.bewer_id=c.bewer_id AND C.precie=15
```

Juan plante **2 puntos primordiales:**
- **Usar producto cartesiano**: Todos los elementos que van en el Where son la linea de entidades que participan para obenter lo requerido.
- **Where-Conexiones**: Las conexiones de todas las entidades van en el Where _coloco las conexiones de cada tabla por medios de las llaves primarias o foraneas hasta llegar al destino_ , y **no debemos olvidar que tambien va la condicion de discriminacion.**

Que cosas puede hacer variar esto **los elementos que requieren nulos**, aqui requiero si o si de hacer un **Join**


![[Pasted image 20230602084422.png]]

![[Pasted image 20230602084436.png]]
