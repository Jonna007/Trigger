# Crear_Triggers

## INVOICE DB

### 1.1 Crear un función para validar que el numero de cedula del cliente tenga 10 números (no letras) en la tabla cliente.

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
![image](https://github.com/user-attachments/assets/2d4a4acf-1e90-4208-b616-218585f0ca1a)


### 1.2 Crear un trigger para validar que el numero de cedula del cliente tenga 10 números (no letras) en la tabla cliente.

```sql
CREATE TRIGGER validar_cedula_cliente_trigger
BEFORE INSERT OR UPDATE ON client
FOR EACH ROW
EXECUTE PROCEDURE validar_cedula_cliente();

```
![image](https://github.com/user-attachments/assets/9cdc0913-9f72-4d9e-8843-ec41ac82c268)

### 2.1 Crear un función para que cada vez que se inserte un nuevo registro en la tabla item se disminuya el stock de la tabla product.

```sql
CREATE OR REPLACE FUNCTION disminuir_stock_producto()
RETURNS TRIGGER AS $$
BEGIN
  -- Disminuir el stock del producto correspondiente
  UPDATE product
  SET stock = stock - NEW.quantity
  WHERE id = NEW.product_id;

  RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```
![image](https://github.com/user-attachments/assets/e6f879eb-cde9-4bd7-bff3-a360f0e9fc38)

### 2.2 Crear un trigger para que cada vez que se inserte un nuevo registro en la tabla item se disminuya el stock de la tabla product.

```sql
CREATE TRIGGER disminuir_stock_producto_trigger
AFTER INSERT ON detail
FOR EACH ROW
EXECUTE PROCEDURE disminuir_stock_producto();
```
![image](https://github.com/user-attachments/assets/ea08a5d3-d9c8-405c-aedf-378b330b480e)

### 3.1 Crear un función para la tabla invoice donde valide que el campo create_at sea del año actual (fecha sistema).
```sql
CREATE OR REPLACE FUNCTION validar_fecha_invoice()
RETURNS TRIGGER AS $$
BEGIN
  IF EXTRACT(YEAR FROM NEW.created_at) != EXTRACT(YEAR FROM CURRENT_DATE) THEN
    RAISE EXCEPTION 'La fecha de creación debe ser del año actual.';
  END IF;

  RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```
![image](https://github.com/user-attachments/assets/21b55f86-aba7-4009-b128-5b197ab81f49)

### 3.2 Crear trigger para la tabla invoice donde valide que el campo create_at sea del año actual (fecha sistema).
```sql
CREATE TRIGGER validar_fecha_invoice_trigger
BEFORE INSERT OR UPDATE ON invoice
FOR EACH ROW
EXECUTE PROCEDURE validar_fecha_invoice();
```
![image](https://github.com/user-attachments/assets/c2fec69a-9215-4b30-9446-4d1403fed85b)

### 4.1 Crear un trigger para la tabla client y validar que el correo tenga un @.
```sql
CREATE OR REPLACE FUNCTION validar_correo_cliente()
RETURNS TRIGGER AS $$
BEGIN
  -- Verificar que el correo contenga un @
  IF POSITION('@' IN NEW.email) = 0 THEN
    RAISE EXCEPTION 'El correo electrónico debe contener un @.';
  END IF;

  RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```
![image](https://github.com/user-attachments/assets/88b14be4-1737-4aee-bb3b-1ada6fc9f6db)


### 4.2 Crear un función para la tabla client y validar que el correo tenga un @.
```sql
CREATE TRIGGER validar_correo_cliente_trigger
BEFORE INSERT OR UPDATE ON client
FOR EACH ROW
EXECUTE PROCEDURE validar_correo_cliente();
```
![image](https://github.com/user-attachments/assets/eb57ebde-5863-4bb4-a9e0-450a348f486c)

