# Ansible GCP

Rol para inicializar una instancia de Google Cloud Platform

## DNS

El rol permite crear registros en Route53 (Amazon). 
Por defecto crea un registro con los siguientes parametros:

```yaml
hostname: "{{ inventory_hostname }}"
domain: "navent.biz"
ip: "{{ gce_private_ip }}"
ttl: 300
type: A
```

Para modificar los registros que se crean, sobreescribir la variable ```dns``` 
con el siguiente formato:
 
```yaml
dns:
  - hostname: "nombre-del-host"   # Valor por defecto {{ inventory_hostname}}
    domain: "dominio.com"         # Valor por defecto "navent.biz"
    ip: "1.2.3.4"                 # Valor por defecto {{ gce_private_ip }}
    ttl: 1000                     # Valor por defecto 300
    type: MX                      # Valor por defecto A
  - hostname: "otro-nombre"
    domain: "otrodominio.com"
    ...
    ...
```

## Discos

El rol permite formatear y montar volumenes adicionales. Configurar la variable ```filesystems```
con el siguiente formato para realizar la acción.

```yaml
filesystems:
  - dev: sbd
    fstype: ext4
    mount_point: /home/ubuntu/data
  - dev: sdc
    fstype: ntfs
    ...
    ...
```