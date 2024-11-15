Aquí tes un documento ben estruturado en formato Markdown que cumpre co solicitado:

```markdown
# Exame Docker

Este repositorio contén as respostas e configuracións necesarias para completar o exame. Cada resposta está xustificada e inclúe exemplos prácticos.

---

## Preguntas e Respostas

### 1. Métodos para 'abrir' unha consola/shell a un contedor en execución
Existen dúas formas principais:
1. **Usando o comando `docker exec`**:  
   ```bash
   docker exec -it <nome_ou_id_contedor> /bin/bash
   ```
   Este comando permite abrir unha consola interactiva no contedor.
2. **Desde unha interface gráfica ou ferramenta de xestión (opcional)**:  
   Facer clic dereito sobre o contedor e seleccionar **Attach Shell**.

---

### 2. Opciones usadas ó arrancar o contedor para interactuar coas súas entradas e saídas
Para interactuar coas entradas e saídas dun contedor, débense usar as opcións `-it`:
```bash
docker run -it <nome_da_imaxe> /bin/bash
```
Isto habilita o modo interactivo e o acceso á consola.

---

### 3. Ficheiro `docker-compose.yml` para que dous contedores se comuniquen entre si nunha rede privada
A continuación, preséntase un exemplo dun ficheiro `docker-compose.yml` que define dous contedores na mesma rede:

```yaml
version: "3.9"
services:
  servizo1:
    image: alpine
    container_name: contedor1
    networks:
      - rede_privada
    command: tail -f /dev/null # Mantén o contedor activo
  servizo2:
    image: alpine
    container_name: contedor2
    networks:
      - rede_privada
    command: tail -f /dev/null # Mantén o contedor activo

networks:
  rede_privada:
    driver: bridge # Tipo de rede
```

---

### 4. Configurar unha IP fixa para un contedor
Para asignar unha IP fixa, engádese o seguinte ao ficheiro anterior:

```yaml
services:
  cliente:
    image: alpine
    container_name: EXAMEN_SRI
    tty: true
    stdin_open: true
    networks:
      P6_network:
        ipv4_address: 172.18.0.3

networks:
  P6_network:
    driver: bridge
    ipam:
      config:
        - subnet: 172.18.0.0/16
```

---

### 5. Comando para obter as IPs dos contedores
Para consultar as IPs asignadas aos contedores, úsase o comando:
```bash
docker network inspect <nome_da_network>
```
No resultado, buscarase a sección `Containers` e a liña de `IPv4Address`:

```json
"Containers": {
  "0512ea52dde7393d1a762a393c78cea5688dbc6c7b5d0bcbcadb826aad59ed7b": {
    "Name": "Examen_bind9",
    "IPv4Address": "172.18.0.2/16"
  }
}
```

---

### 6. Funcionalidade do apartado `ports` en Docker Compose
O apartado `ports` mapea os portos do host cos do contedor. Exemplo:
```yaml
ports:
  - "54:53/udp"  # Porto UDP 53 mapeado a 54 no host
  - "54:53/tcp"  # Porto TCP 53 mapeado a 54 no host
  - "127.0.0.1:953:953/tcp" # Só accesible dende localhost
```

---

### 7. Rexistro CNAME: Definición e exemplo
O rexistro CNAME crea un alias a un nome canónico. Por exemplo:
```text
CNAME: Alias de www, permite que alias.Examen_SRI.int apunte a www.Examen_SRI.int.
```

---

### 8. Evitar a perda de configuración dun contedor DNS
Para garantir que o contedor DNS manteña a súa configuración, engádese:
```yaml
restart: always
```
Isto asegura que o contedor se reinicie automaticamente en caso de erro ou reinicio.

---

### 9. Engadir unha zona DNS `tendaelectronica.int`
Para engadir a zona DNS con `www`, `owncloud` e un rexistro de texto, creamos un ficheiro chamado `tendaelectronica.int.zone`:

```text
$TTL 86400
@   IN  SOA ns1.tendaelectronica.int. admin.tendaelectronica.int. (
        2024111501 ; Serial
        3600       ; Refresh
        1800       ; Retry
        1209600    ; Expire
        86400      ; Minimum TTL
)
    IN  NS  ns1.tendaelectronica.int.
www IN  A   172.16.0.1
owncloud IN CNAME www
@   IN  TXT "1234ASDF"
```

Para probalo:
1. Levantamos os contedores:
   ```bash
   docker-compose up
   ```
2. Consultamos os rexistros:
   ```bash
   dig @<ip_dns> www.tendaelectronica.int
   dig @<ip_dns> owncloud.tendaelectronica.int
   dig @<ip_dns> tendaelectronica.int TXT
   ```
3. Verificamos os logs:
   ```bash
   docker logs <nome_ou_id_do_dns>
   ```

---

## Resultados

Os pasos anteriores permiten configurar e probar os contedores segundo as especificacións do exame.
```

