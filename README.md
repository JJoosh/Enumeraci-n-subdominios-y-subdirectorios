# Enumeraci-n-subdominios-y-subdirectorios

## *Subdominios*

> Un subdominio te permite dividir tu sitio web en varias secciones. Para ello, el nombre del subdominio se inserta en la dirección **delante del nombre del dominio (dominio de nivel superior) y separado por un punto**. La siguiente imagen ilustra esta estructura:

![image](https://github.com/JJoosh/Enumeraci-n-subdominios-y-subdirectorios/assets/122099216/76461105-473a-499e-aea0-17b57d340c62)


Muy bien ya comprendido que es un subdominio veamos para que nos puede servir saberlo pues enumerar un subdominio es una fase crucial en la seguridad informática 

Al identificar los subdominios vinculados a un dominio principal, un atacante podría obtener información valiosa para cada uno de estos, lo que le podría llevar a encontrar **vectores de ataque potenciales**. Por ejemplo, si se identifica un subdominio que apunta a un servidor web vulnerable, el atacante podría utilizar esta información para intentar explotar la vulnerabilidad y acceder al servidor en cuestión.

Existen varias herramientas para enumerar subdominios, como las pasivas y las activas, las pasivas se basa en todo lo que este publico en internet y las activas son las que hay un poco de interacción con el servidor, es por eso que cuando usamos las activas debemos ser lo más precavido o dejamos un rastro y eso nos puede costar muy caro 

A continuación, algunas herramientas que hay para este tipo de actividad son: 

- [**Phonebook**](https://phonebook.cz/) (Herramienta pasiva)
- [Intelx](https://intelx.io/) (Herramienta pasiva)
- [CTFR](https://github.com/UnaPibaGeek/ctfr) (Herramienta pasiva)
- [Gobuster](https://github.com/OJ/gobuster)(Herramienta activa)
- [Wfuzz](https://github.com/xmendez/wfuzz) (Herramienta activa)
- [Sublist3r](https://github.com/huntergregal/Sublist3r) (Herramienta pasiva)

En esta ocasión procederemos a usar primero el CTFR porque muestra la información en consola y a criterio personal me parece mejor 

para ello lo que haremos será 

```bash
git clone https://github.com/UnaPibaGeek/ctfr
cd ctfr
pip install -r requirements.txt
python3 ctfr.py -d <dominio>
```

Nos desplegara algo como esto 

![image](https://github.com/JJoosh/Enumeraci-n-subdominios-y-subdirectorios/assets/122099216/22a42dfe-f561-4622-9a24-0b40a19752f4)


Recalco esto es de forma **pasiva** es todo lo que es publico en internet 

Muy bien ahora para hacerlo de forma pasiva, a mi en lo personal me gusta mucho 
wfuzz porque primero trae colores cuando muestra en pantalla y segundo porque nos permite filtrar por códigos de estado haciendo que no se vea tan molestoso a la hora de visualizar los resultados, usaremos el siguiente comando

```bash
wfuzz -c -t 20 --hc=404 -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt -H "Host: FUZZ.uady.mx" https://uady.mx/ 
```


En este comando hacemos lo siguiente:

* `-c`: Este parámetro nos permite visualizar todo el resultado con colores, recuerda siempre que una herramienta nos permita visualizar con colores aplíquenlo
* `-t`: Este comando significa threads o sea hilos para que aplique de forma paralela los subdominios de nuestra wordlists haciendo más rápido el escaneo 
* `--hc=404`: Este significa hide code o sea no me muestres los codigo de estado que sean 404 o no encontrados, nos permite agilizar un poco más el escaneo y no mostrar basura 
* `-w`: Es para indicar la Wordlist en este caso hemos usado la lista de SecLists dejo la repo aca: [SecLists](https://github.com/danielmiessler/SecLists.git)
* `-H "Host: FUZZ.uady.mx"`: esta opción especifica la cabecera HTTP que se enviará en cada solicitud. La parte "FUZZ" será reemplazada por las palabras clave de subdominios de la lista proporcionada
* `https://uady.mx/`: esta es la URL objetivo sobre la cual se realizarán las solicitudes de enumeración de subdominios.


Muy como ultima herramienta veremos Sublist3r, una herramienta pasiva pero que a veces sale bunena información, para instalarlo haremos lo siguiente:
nos diregimos a la carpeta opt y hacemmos lo siguiente:

``` bash
sudo git clone https://github.com/huntergregal/Sublist3r.git
cd Sublist3r/
sudo python3 setup.py install 
pip3 install -r requirements.txt  
python3 sublist3r.py -d tinder.com
```

Si todo salió bien nos mostrar algo como esto: 

![image](https://github.com/JJoosh/Enumeraci-n-subdominios-y-subdirectorios/assets/122099216/485e97b4-3607-49c0-a8fd-453d9de97495)

____

## *Subdirectorios*

> Un subdirectorio es una carpeta secundaria dentro de un directorio principal en un servidor web. Es decir, es un espacio dentro de una carpeta principal que contiene otros archivos y carpetas.
> 

![image](https://github.com/JJoosh/Enumeraci-n-subdominios-y-subdirectorios/assets/122099216/9ed1e483-849e-44aa-87d5-64c7399d901a)


Y como vimos en el apartado de subdominios, si un hacker busca subdirectorios que contienen información sensible o alguna vulnerabilidad, este podría aprovechar de estas para hacer el mal o en su caso reportar y corregir el problema

Bine, ahora existen varias herramientas que nos permiten hacer estas, tal esta el caso de wfuzz, gobuster, dirb y dirbuster

les dejo link de las herramientas: 

- [Gobuster](https://github.com/OJ/gobuster)(Herramienta activa)
- [Wfuzz](https://github.com/xmendez/wfuzz) (Herramienta activa)
- [Dirb](https://github.com/v0re/dirb)  (Herramienta activa)
- [Dirbuster](https://www.kali.org/tools/dirbuster/) (Herramienta activa)

Nosotros estaremos viendo las herramientas wfuzz, dirb y gobuster 

#### *wfuzz*

Para poder utilizar wfuzz hacemos lo mismo que en subdomnios pero cambiando algunos valores

```bash
wfuzz -c --hc 404 -t 200 -w <wordlists> <ip o dominio/FUZZ> 
```

Este es un comando muy básico pero muy útil, recuerda siempre hay que poner la palabra FUZZ donde queremos que busque los subdirectorios web

Ejemplo:

![image](https://github.com/JJoosh/Enumeraci-n-subdominios-y-subdirectorios/assets/122099216/28c060cb-000b-4548-8741-99c3688da9d0)


___
#### Dirb

Para usar dirb es un poco más sencillo que el anterior, normalmente solemos usarlo cundo creemos que no hay tantos direcotorios para descubrir y queremos rápido las cosas

``` bash
dirb url
```

**Nota:** `tener una buena wordlist nos asegurar que los resultados sean muchas mejores, siempre podemos probar buscar en github repositorios de wordlists de subdirectorios más comunes `

Ejemplo:

![image](https://github.com/JJoosh/Enumeraci-n-subdominios-y-subdirectorios/assets/122099216/652d421c-d2a9-46cf-a510-309fd23854d3)

____ 

#### Dirbuster 

Bien para los que les gusta más la interfaz que la terminal existe esta alternativa llamada Dirbuster

para ellos bastara con poner en nuestra terminal dirbuster y nos abrirá una interfaz como esta, solo bastara colocar los datos correspondientes como la wordlists la url etc...

debería verse algo así: 

![image](https://github.com/JJoosh/Enumeraci-n-subdominios-y-subdirectorios/assets/122099216/34af0ad0-0393-44fd-be1a-c614652a6f9e)

![image](https://github.com/JJoosh/Enumeraci-n-subdominios-y-subdirectorios/assets/122099216/5e9f87d8-58b5-447e-91c0-0996bbee618f)

