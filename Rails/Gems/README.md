# Gems

Esta sección estara concentrada en gemas populares y mi experiencia en su uso, asi como tambien los escenarios en los cuales son utiles y se puede usar.

* [Scenic](https://github.com/enmanuelm19/CheatSheets/blob/master/Rails/Gems/scenic.md)
: Esta gema tiene la finalidad de facilitar la creación de vistas de bases de datos, posible escenario:
  Cuando necesitamos tener disponibles datos que compartidos entre varias tablas, supongamos que para acceder a esa data, en el modelo
  tengamos que realiza muchas consultas `joins(:models).where(conditions: values).where.not(condition: condition).group(:attribute)`
