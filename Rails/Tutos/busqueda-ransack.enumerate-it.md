####  Buscar por valor de Enum manejados por Enumerate It

En una de mis asignaciones como programador se me solicito que le colocara unos filtros a una tabla de unos registros, pero que la busqueda fuera en back, aqui es donde entra la gema **Ransack**, esta gema provee una sintaxis sencilla para realizar busquedas.

Como trabajaba con una API era necesario que desde el front se armara el JSON correspondiente para realizar la busqueda, en general realizar busquedas con la gema es muy sencillo, solo hace falta hacer uso del metodo que este nos dispone:

> Person.ransack(params[:q]).result

Los parametros que necesita ransack para la busqueda vienen formados de la siguiente forma:

```Javascript
  q: {
    field_name_predicate: value,
    first_name_eq: 'Enmanuel',
    mail_cont: 'some_mail@mail.com'
  }
```

Como podemos ver solo hace falta indicar el nombre del campo a buscar y el predicado, en este ejemplo colocamos **eq** y **cont**, en el primer caso busca por igualdad y el segundo por contenido (LIKE).

Sencillo hasta este punto, el problema se presenta cuando la tabla **Person** hace uso de enums para manejar ciertos estados, por lo tanto tener los estados **active:1, inactive:2, disabled:3...** realiza la persistencia de solo los numeros dentro de la base de datos.

Como el filtro requeria que se buscara por nombre, ejemplo: **activo**, no habia manera de comparar el nombre con el valor numerico que tenia en la base de datos, aqui es donde entra la magia de los **ransacker**.

En el modelo **Person** colocamos lo siguiente para procesar los enum:

```Ruby
  ransacker :state, formatter: proc {|v|
    value = I18n.backend.send(:translations)[:es][:enumerations][:state].key(v).to_s.upcase
    State.value_for(value)
  }
```
Esto le da formato al query antes de realizar la busqueda, la primera linea dentro del PROC asigna a la variable **value** la traducción del parametro pasado, ejemplo: En la busqueda se coloca **Activo** como parametro, en mis archivos locales tengo definido que la traducción de active es Activo, por lo tanto esto realiza la traducción inversa(busca el key en vez del valor), y me trae **ACTIVE**, con enumerate it busco entoces el valor del enum `State.value_for(value)` lo que me retorna 1, y la consulta entonces queda `SELECT * FROM people WHERE state = 1`, dandome exactamente lo que necesitaba.
