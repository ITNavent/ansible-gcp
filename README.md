# Ansible GCP

Rol para inicializar instancias en Google Cloud Platform

## Instancia

El rol permite crear instancias. Las siguientes variables con sus valores por defecto están disponibles para crear instancias ([Documentación](http://docs.ansible.com/ansible/latest/gce_module.html)):

```yaml
instance_name: ""                       # Nombre base para las instancias (ej: "test" produce las instancias "test-1", "test-2", etc...
instance_num_instances: 0               # Numero de instancias a crear
instance_machine_type: n1-standard-1    # Flavor de la instancia
instance_tags: no-ip, zabbix-agent      # Tags de la instancia. Google los precede con el prefijo "tag_"
instance_zone: us-east1-c               # Zona de la instancia
instance_preemptible: false             # Crear instancia preemptive
instance_metadata: {}                   # Metadata de la instancia
instance_ssh_key: "ubuntu:{{ lookup('file', lookup('env', 'SSH_PRIVATE_KEY')) }}"     # Clave SSH para la instancia

instance_network: default               # Red
instance_subnetwork: ""                 # Subred
instance_external_ip: none              # IP externa (ephemeral, none, 'external-ip-name', 123.123.123.123)
instance_ip_forward: false              # Redirigir paquetes

instance_image: ubuntu-1604-xenial      # Imagen para el disco de booteo
instance_disk_size: 10                  # Tamaño para el disco de booteo
instance_disk_auto_delete: true         # Eliminar el disco de booteo al eliminar la instancia 
instance_persistent_boot_disk: true     # Crear disco de booteo 
instance_disks: []                      # Discos adicionales
```

El formato para la variable de discos adicionales es:

```yaml
instance_disks:
  - disk_type: pd-standard                      # Tipo de disco (pd-standard, pd-ssd)
    image: ""                                   # Imagen para inicializar el disco
    mode: READ_ONLY                             # Modo de montaje del disco (READ_ONLY, READ_WRITE)
    name: "persistent-{{ instance_disk_num }}"  # Nombre del disco (Se lo prefija con "{{ instance_name }}-{{ intance_num }}-")
    size_gb:  10                                # Tamaño del disco en GB
    fstype: ext4                                # Filesystem
    mount_point: /home/ubuntu/data              # Punto de montaje
```

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

## Hostname

El rol establece el hostname del equipo. Para configurarlo darle valor a las siguientes variables:
```yaml
hostname: "nombre-del-host"     # Valor por defecto {{ inventory_hostname }}
domain: "dominio.com"           # Valor por defecto "navent.biz"
```