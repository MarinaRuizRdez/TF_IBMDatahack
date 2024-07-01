Informe de marketing de DH - IBM Datahack School
Proyecto DH Consultores de Marketing - IBM Datahack 

Curso Power BI (Abril-2024).
 
Autora: Marina Ruiz Rodriguez 
 

ETL

Conjunto de datos: https://www.kaggle.com/datasets/rodsaldanha/arketing-campaign?resource=download

Limpieza y transformación de datos.

Se cambia los tipos de columnas (texto, moneda y numero entero).

Se renombra los encabezados al español (Ejemplo: Marital Status por estado civil).

Remplazo de nombre de valores, ejemplo null =0 o  single = Soltera/o.



Filtros: Para eliminarlos de las tablas de dimensiones posteriores y del cuadro de mando. Evitando así eliminar información de origen.

Table.SelectRows(#"Remplazar PhD", each [Salario Anual] <> 666666)

Table.SelectRows(#"Filtro <> 666666", each [Año Nacimiento] > 1900)

= Table.SelectRows(#"Filtro >1900", each [Salario Anual] <> 0)



Se crean tablas de dimensiones. No se han combinado columnas puesto que el proyecto no lo requería. Fact: Ventas y Dim: Campañas, Clientes, Productos, Plataformas. La tabla calendario de crea en DAX.

Se elimina de las tablas las columnas : Z_CostContact y Z_Revenue.

Modelado:



Modelo de estrella: Una tabla de hechos y 4 dimensiones.



DAX:

d_Calendar = 

VAR _min = MIN(d_Clientes[Fecha Alta])

VAR _max = MAX(d_Clientes[Fecha Alta])

RETURN

    ADDCOLUMNS(
    
        CALENDAR(_min, _max),

        "Año #", YEAR([Date]),

        "Mes #", MONTH([Date]),

        "Día #", DAY([Date]),

        "Nombre día", FORMAT([Date], "dddd"),

        "Nombre Mes", FORMAT([Date], "mmmm"),

        "Short Date", FORMAT([Date], "dd/mm/yyyy"),

        "Número Mes", MONTH([Date]),

        "Comienzo Nombre día", WEEKDAY([Date], 2)

        )



Se han usado medidas como SUM, DIVIDE, CALCULATE, MIN o MAX,  AVERAGE, RANKX, COUNTROWS. Están organizadas en carpetas por tipos: Totales, Promedios, Máximas, Ranking.

Visualizaciones:



Gráficos de líneas.
Gráficos de barras.
Gráficos de anillos.
Tablas.
Matrices.
Paneles de filtro.
Marcadores y botones de navegación


Cuadro de mandos:



Índice: Resumen de datos (porcentaje aceptación campañas, ranking de clientes con mas Nº de compras, clasificación por clases, graficas de crecimiento de ventas y comportamiento de inscripción clientes.



Campañas: Se genera Ranking en tabla, donde se muestra el comportamiento y aceptación de los clientes. El ranking de clientes con mayor Nº de campañas aceptadas, no se ha podido generar desempates (año de nacimiento u/o Alta, se repetían igualmente los clientes, se deja la medida mas simple)



Clientes: Los clientes se clasifican por edades y generaciones, clases sociales: Baja <= 19000 , Media  <= 50000, Alta  >= 50001 , totales de hijos (niños y adolescentes) al igual que con/sin hijos, estado civil, educación.

Se analiza  el consumo por tipo social y personal.



Ventas:  Se analiza el comportamiento de las compras según su estado social y personal.





Se agregan columnas personalizadas y condicionales. En las tablas que se requiere se anula la dinamización.
