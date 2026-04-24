# Automatización de scraping de datos en redes sociales y procesados por un Agente IA especializado en Marketing

## MODULO #1: Entrada de Datos

`En este modulo del escenario primeramente el nodo webhook recibe la url de la publicación que se desea hacer scraping, bueno luego`

![[Pasted image 20250716214657.png]]

El nodo "WebHook" es el primer nodo del escenario, se encarga de recibir en el body especialmente este tipo de estructura en su array:

```
{
	"url": "http://www.facebook.com/ejemplo",
	"categoria": "facebook"
}
```

Luego el nodo "EstructuraURL" normaliza los datos en una estructura mas amigable a futuro, pasa por un switch estos datos y se filtran correspondiendo a la categoría que proviene esta url,
categorías posibles:

```
[
	{
		"categoria":"facebook"
	},
	{
		"categoria":"instagram"
	},
	{
		"categoria":"tiktok"
	}
]
```

## Modulo #2: Scraping de Redes Sociales

`En este parte del workflow ya empezamos a realizar algunas automatizaciónes, el nodo "Envia Solicitud al actor" es el que se encarga de hacer la peticion a Apify para utilizar los servicios de scraping que nos ofrecen, al final estos datos se almacenan en una hoja de Google Sheets.

![[Pasted image 20250716220008.png]]

El primer nodo "Envia la solicitud al actor" se envian ciertos datos mediante el nodo HTTP, se envia la solicitud a la ruta del url y luego por body pasa los otros datos del siguiente JSON:

```
{
	headers:
	{
		"url": "https://api.apify.com/v2/acts/alien_force~facebook-posts-comments-scraper/runs"
	},
	{
	    "comments_limit": 0,
	    "post_urls": [
	        {
	            "url": "{{ $json.link }}",
	            "method": "GET"
	        }
	    ]
	}
}
```

(*Claro que se debe enviar en la cabecera el token que nos proporciona Apify  para usar sus servicios y tambien los parametross de Content-Type: application/json*) 

Luego en el segundo nodo "Scrap" como se visualiza en la imagen realiza la ejecución y espera hasta que el scraping se realiza, si en el resultado esta el estado asi:

```
{
	"Success": "Proccess"
}
```

que vaya por false en el siguiente nodo IF y que vuelva a esperar adicionalmente antes de volver a consultar el nodulo.
Si es "TRUE" pues se piden los datos en el siguiente nodo HTTP "Resultados del Scrap" el cual devuelve un json con los datos resultantes del scraping, luego un nodo code normaliza los datos para luego ser registrado con el nodo de Google Sheets en la hoja de "Facebook-post"
