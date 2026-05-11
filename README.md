# Taller práctico 04: Desafío de Consultoría "Gobernanza Digital"

# Análisis de Mercado y Selección

## 3 años

|  | Odoo | SAP S/4HANA | Zoho One |
| :---- | ----- | :---- | :---- |
| Coste de licencias/suscripción | 648€/usuario \[4\] | 648€/usuario\[5\] | 1332€/usuario \[3\] |
| Coste de implantación | 4000€ | 4000€ | 4000€ |
| Coste operativo |  |  |  |

<img width="802" height="900" alt="Captura" src="images/descarga.png" />

# MATRIZ DE PERMISOS\[1\]

Diseña la matriz de permisos para los siguientes roles, asegurando el **Principio de Mínimo Privilegio**:

* **Administrador:** Acceso total.  
* **Comercial:** Solo ve sus clientes y presupuestos (Record Rules).  
* **Operario de Almacén:** Solo ve stock y albaranes de entrada/salida.  
* **Contable:** Puede mirar facturas pero no puede modificar el stock.

|  | Permisos | Administrador | Comercial | Operario en almacén | Contable |
| :---- | :---- | :---- | :---- | :---- | :---- |
| Ventas/Clientes | C/L/A/B | Sí/Sí/Sí/Sí | Solo Propios | No Acceso | Solo Lectura |
| Presupuestos/Pedidos | C/L/A/B | Sí/Sí/Sí/Sí | Solo Propios | No Acceso | Solo Lectura |
| Inventario/Stock | C/L/A/B | Sí/Sí/Sí/Sí | No Acceso | Sí/Sí/No/No | Solo Lectura |
| Albaranes | C/L/A/B | Sí/Sí/Sí/Sí | No Acceso | Sí/Sí/Sí/No | Solo Lectura |
| Facturas/Pagos | C/L/A/B | Sí/Sí/Sí/Sí | Lectura | No Acceso | Sí/Sí/Sí/No |

**C:Crear**  
**L:Leer**  
**A:Actualizar**  
**B:Borrar** 

# MANUAL DE DESPLIEGUE

## Bloque C: Documentación de Explotación (CE i)

Siguiendo la norma **ISO/IEC 26514**, redacta un breve **Manual de Despliegue** para que el responsable de IT de la empresa pueda levantar el sistema en caso de caída. Debe incluir:

1. El fragmento de *docker-compose.yml* necesario.  
2. El comando para realizar un backup de la base de datos PostgreSQL.

la norma **ISO/IEC 26514** dicta los requisitos para el diseño y el desarrollo de la documentación que hace el usuario sobre el software como parte de los procesos del ciclo de vida \[2\]

es normal que haya caídas del servidor de la empresa, voy a tratar la recuperación en caso de caidas con un comando de PostgreSQL pg\_dump \-U postgres \-d db12 \> backup.sql 

## 1.docker desktop

Es un software de código abierto que permite el uso de herramientas en los contenedores linux, siendo más eficiente y cómodo que las máquinas virtuales a la hora del desarrollo de software, con docker vamos a desplegar nuestra aplicación.

## 2\. Docker-compose.yml

Este archivo define la estructura para el despliegue de la aplicación, después se ejecuta con el comando docker-compose up \-d 

services:  
  odoo:  
    image: odoo:latest  
    container\_name: odoo  
    restart: unless-stopped  
    depends\_on:  
      \- db  
    ports:  
      \- "8200:8069"  
    volumes:  
      \- odoo-data:/var/lib/odoo  
      \- ./config:/etc/odoo  
      \- ./addons:/mnt/extra-addons  
    environment:  
      \- HOST=db  
      \- USER=odoo  
      \- PASSWORD=odoo  
    command: odoo \-d odoo \--db\_user=odoo \--db\_password=odoo \-i base

  db:  
    image: postgres:16.0  
    container\_name: db  
    restart: unless-stopped  
    environment:  
      \- POSTGRES\_DB=odoo  
      \- POSTGRES\_PASSWORD=odoo  
      \- POSTGRES\_USER=odoo  
      \- PGDATA=/var/lib/postgresql/data/pgdata  
    volumes:  
      \- db-data:/var/lib/postgresql/data

volumes:  
  odoo-data:  
  db-data:  
El resultado debería de ser el CRM y el centro de base de datos. \[6\]

# REFERENCIAS

\[1\] [https://ventor.tech/odoo/odoo-access-rights/](https://ventor.tech/odoo/odoo-access-rights/)  
\[2\] [https://estandaresti.wordpress.com/2016/12/20/isoiec-265142008/](https://estandaresti.wordpress.com/2016/12/20/isoiec-265142008/)  
\[3\] [https://www.zoho.com/one/pricing/](https://www.zoho.com/one/pricing/)

\[4\] [https://www.odoo.com/es\_ES/pricing](https://www.odoo.com/es_ES/pricing)

\[\]\[\]  
\[5\][https://www.cronomia.com/software/sap-s4-hana](https://www.cronomia.com/software/sap-s4-hana)  
\[6\] https://docs.google.com/presentation/d/1IfBdJSnPdbwJxgf1Fksk7Jr8ABuNzdg5kqkclc188SE/edit?slide=id.g3d953a33658\_1\_0\#slide=id.g3d953a33658\_1\_0  
