# Crear_Triggers

## INVOICE DB

### 1. Crear un función y un trigger para validar que el numero de cedula del cliente tenga 10 números (no letras) en la tabla cliente.

```sql
CREATE OR REPLACE FUNCTION validar_cedula_cliente()
RETURNS TRIGGER AS $$
BEGIN
  IF LENGTH(NEW.nui) != 10 OR NEW.nui !~ '^[0-9]+$' THEN
    RAISE EXCEPTION 'El número de cédula debe tener exactamente 10 números.';
  END IF;

  RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```
![image](https://github.com/user-attachments/assets/577916fb-3b9d-467c-9de9-b481a9cea153)



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
