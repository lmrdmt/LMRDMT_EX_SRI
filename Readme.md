Crea un repositorio para contestar todo o exame.
Este repositorio ten que conter tódo-los ficheiros necesarios para xustifica-la túas respostas.

Contesta as seguintes preguntas, xustificándoas, nun README.md

1. Explica métodos para 'abrir' unha consola/shell a un contenedor en execución.
Para acceder a el, podemos utilizar o comando `docker exec -it u1 /bin/bash` ou facer clic dereito sobre o contedor e seleccionar **Attach Shell**.
2. No contenedor anterior (en execución), qué opciones tes que ter usado ó arrincalo para poder interactuar coas súas entradas e salidas

	la opción -it "nombre del contenedor" /bin/bash
	
3. Cómo sería un ficheiro docker-compose para que dous contenedores se comuniquen entre si nunha rede só deles?

services:  # Define os servizos (contenedores)
  
  <Nome_do_contedor>:
    image: <Imaxe_a_utilizar>
    container_name: <Nome_especifico>
    networks: 
      - <Nome_da_rede>
    command: tail -f /dev/null #Este comando fai que se mantenga activo o contenedor
  <Nome_do_contedor>:
    image: <Imaxe_a_utilizar>
    container_name: <Nome_especifico>
    networks: 
      - <Nome_da_rede>
    command: tail -f /dev/null #Este comando fai que se mantenga activo o contenedor
 
networks:  # Definición das redes
  <nome_da_rede>:  # Nome da rede que estamos a crear
    driver: <tipo_de_rede> 


4. Qué tes que engadir ó ficheiro anterior para que un contenedor teña unha IP fixa?

cliente:
    image: alpine #Imaxe do cliente
    container_name: EXAMEN_SRI #Nome do container
    tty: true
    stdin_open: true
    networks:
      P6_network: #Nome da rede
        ipv4_address: 172.18.0.3 #IP que lle otorgamos ao cliente


5. Qué comando de consola podo usar para sabe-las ips dos contenedores anteriores?
comando:

docker network inspect <nome_da_network>

Buscaremos na resposta o apartado `Containers` e a liña de `IPv4Address`:

"Containers": {
    
          "0512ea52dde7393d1a762a393c78cea5688dbc6c7b5d0bcbcadb826aad59ed7b": {
                "Name": "Examen_bind9",
                "EndpointID": "1ca7b6de3e6355e1c9aa78c60c1c6ffd3e4b1ac67a5b989314de2be5c4247c2d",
                "MacAddress": "02:42:ac:12:00:02",
                "IPv4Address": "172.18.0.2/16",
                "IPv6Address": ""
            }

6. Cál é a funcionalidade do apartado "ports" en docker compose?
Establecer el mapeo de puertos.


ports:
      #Mapeo dos portos host:contenedor
      - 54:53/udp
      - 54:53/tcp
      - 127.0.0.1:953:953/tcp #Neste só pertmite conexións desde localhost


7. Para qué serve o rexistro CNAME? Pon un exemplo
Este tipo de registros siempre tienen 16 bits
Un CNAME define un alias a un nombre canónico:

CNAME: Alias de www, permite que alias.Examen_SRI.int apunte a www.Examen_SRI.int. 

8. Cómo podo facer para que a configuración dun contenedor DNS non se borre se creo outro contenedor?

restart: always #Esta opción indica que ó contenedor debe reiniciarse en ciertas condiciones


9. Engade unha zoa tendaelectronica.int no teu docker DNS que teña:
- www á IP 172.16.0.1
- owncloud sexa un CNAME de www
- un rexistro de texto có contido "1234ASDF"
Comproba que todo funciona có comando "dig"
Mostra nos logs que o servicio funciona ben usando a saída da terminal ó levantar o compose ou có comando "docker logs [nomeContenedorOuID]"

[+] Running 2/0
 ✔ Container DNS_EXAMEN      Created                                                                       0.0s 
 ✔ Container Cliente_EXAMEN  Created                                                                       0.0s 
Attaching to Cliente_EXAMEN, DNS_EXAMEN

(o apartado 9 realízase na máquina virtual)


13061	IN	NS	f.root-servers.net.
.			13061	IN	NS	c.root-servers.net.
.			13061	IN	NS	i.root-servers.net.
.			13061	IN	NS	b.root-servers.net.
.			13061	IN	NS	d.root-servers.net.
.			13061	IN	NS	g.root-servers.net.
.			13061	IN	NS	e.root-servers.net.
.			13061	IN	NS	a.root-servers.net.
.			13061	IN	NS	h.root-servers.net.
.			13061	IN	NS	k.root-servers.net.
.			13061	IN	NS	l.root-servers.net.
.			13061	IN	NS	m.root-servers.net.
.			13061	IN	NS	j.root-servers.net.





