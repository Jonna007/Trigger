# Crear_Triggers

## INVOICE DB

### 1. El numero total de facturas realizadas por cada cliente.(nombre_cliente | direccion | nro_facturas)

```sql
SELECT 
    c.fullname AS nombre_cliente,
    c.address AS direccion,
    COUNT(i.id) AS nro_facturas
FROM 
    client c
JOIN 
    invoice i ON c.id = i.client_id
GROUP BY 
    c.fullname, c.address;
```
![image](https://github.com/Jonna007/Queries_with_Subqueries/assets/146044709/985fb668-0ce0-4d02-b172-b0b311713e9c)


### 2. Listar nombre y correo de los clientes junto a su compra mas cara realizada. (nombres |  correo   | total_mas_alto)
```sql
SELECT 
    c.fullname AS nombres,
    c.email AS correo,
    (SELECT MAX(i.total) 
     FROM invoice i 
     WHERE i.client_id = c.id) AS total_mas_alto
FROM 
    client c;
```
![image](https://github.com/Jonna007/Queries_with_Subqueries/assets/146044709/89ba1b09-9805-4ecd-a865-77bd2eeb3a92)

### 3. Listar las facturas donde sus totales sean mayores al promedio de las facturas. (fecha_factura | total)
```sql
SELECT 
    i.create_at AS fecha_factura,
    i.total
FROM 
    invoice i
WHERE 
    i.total > (SELECT AVG(total) FROM invoice);
```
![image](https://github.com/Jonna007/Queries_with_Subqueries/assets/146044709/dea4b255-d9f7-42f9-8534-77a547350b4e)

## EVENT DB

### 1. Crear dos ejemplo con la base de datos event. Uno con subconsulta en SELECT y otro con subconsulta  en WHERE

### 1.1 Ejemplo subconsulta en SELECT
```sql
SELECT 
    e.event_id,
    e.event_name,
    (SELECT COUNT(*) 
     FROM registrations r 
     WHERE r.event_id = e.event_id) AS total_registrations
FROM 
    events e;
```
![image](https://github.com/Jonna007/Queries_with_Subqueries/assets/146044709/c77bf847-64d3-46fa-b11d-6618dc640dae)

### 1.2 Ejemplo subconsulta en WHERE
```sql
SELECT 
    e.event_id,
    e.event_name,
    e.event_date
FROM 
    events e
WHERE 
    e.event_id IN (SELECT r.event_id 
                   FROM registrations r 
                   WHERE r.member_id = 1);
```
![image](https://github.com/Jonna007/Queries_with_Subqueries/assets/146044709/2a50bb32-b2ff-4916-9120-6717ccf02045)