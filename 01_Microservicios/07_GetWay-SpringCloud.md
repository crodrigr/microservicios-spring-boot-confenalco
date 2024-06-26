# Spring Cloud Getway

<br>

Spring Cloud **Gateway** y **Zuul** son dos gateways/API gateways utilizados en el ecosistema de Spring Cloud para enrutar y filtrar solicitudes en arquitecturas de microservicios. A continuación, se presenta una comparación entre ambos:

**Spring Cloud Gateway:**

  - Spring Cloud Gateway es el gateway/API gateway desarrollado específicamente para Spring Cloud.
  - Está construido sobre Spring WebFlux y el modelo de programación reactivo.
  - Utiliza una arquitectura funcional y reactiva, lo que permite un alto rendimiento y escalabilidad.
  - Proporciona una configuración basada en rutas y un enfoque orientado a predicados y filtros.
  - Ofrece una mayor flexibilidad y extensibilidad gracias a su enfoque funcional y la posibilidad de escribir lógica personalizada utilizando funciones lambda y el operador Reactor.

**Zuul:**

  - Zuul es un gateway/API gateway desarrollado originalmente por Netflix y ampliamente adoptado en el ecosistema de Spring Cloud.
  - Se basa en el modelo de programación Servlet.
  - Utiliza una arquitectura más tradicional basada en filtros.
  - Proporciona una configuración basada en rutas y un enfoque orientado a filtros predefinidos y personalizados.
  - Tiene un ecosistema más maduro y una amplia comunidad de usuarios, con características y extensiones adicionales disponibles.
  
  Algunas diferencias clave entre Spring Cloud Gateway y Zuul son:

  - **Modelo de programación:** Spring Cloud Gateway utiliza un modelo de programación funcional y reactivo, mientras que Zuul utiliza un modelo basado en Servlet.
  - **Rendimiento:** Debido a su enfoque reactivo y al uso de Spring WebFlux, Spring Cloud Gateway puede ofrecer un mejor rendimiento en comparación con Zuul.
  - **Flexibilidad:** Spring Cloud Gateway proporciona una mayor flexibilidad y extensibilidad gracias a su enfoque funcional y reactivo.
  - **Ecosistema:** Zuul tiene un ecosistema más maduro y ha sido ampliamente adoptado, con una mayor cantidad de características y extensiones disponibles.
    
En resumen, tanto Spring Cloud Gateway como Zuul son opciones viables para implementar gateways/API gateways en el ecosistema de Spring Cloud. Spring Cloud Gateway se destaca por su enfoque reactivo y funcional, así como su flexibilidad y escalabilidad. Por otro lado, Zuul tiene un ecosistema más establecido y ofrece una opción más madura y ampliamente adoptada. La elección entre ambos dependerá de los requisitos específicos del proyecto y las preferencias técnicas.

Que son los predicados y filtros:

  - **Predicados:** Los predicados se utilizan para tomar decisiones de enrutamiento basadas en características específicas de la solicitud entrante, como la ruta, los encabezados, los parámetros, etc. Los predicados determinan si una ruta en particular se debe aplicar a una solicitud entrante. Algunos ejemplos de predicados son los predicados de ruta, de encabezado, de método y de parámetro de consulta.

  - **Filtros:** Los filtros se utilizan para realizar transformaciones y operaciones en las solicitudes y respuestas que pasan a través del Gateway. Los filtros pueden modificar, agregar o eliminar encabezados, modificar la solicitud o respuesta, realizar autenticación, agregar métricas, entre otras operaciones. Spring Cloud Gateway proporciona una variedad de filtros predefinidos que se pueden configurar y también permite crear filtros personalizados según tus necesidades.

   - **uri:** se refiere a la URL de destino a la que se enviará la solicitud una vez que se cumplan los predicados y se haya realizado el enrutamiento adecuado. Es la dirección a la que se reenviará la solicitud después de aplicar cualquier transformación o filtro configurado.
     La URI se define en la configuración de la ruta de Spring Cloud Gateway y puede ser una dirección URL completa o un URI 
relativo. Aquí tienes un ejemplo de configuración de ruta con la URI definida:

<br>
<br>
<br>

## 1. Spring Cloud Getway - proyecto

En el spring initializr se realiza la configuración del proyecto con las siguentes dependencias: Gateway, Eureka Client, Web Tools

### 1.1 Configuración de proyecto

![image](https://github.com/crodrigr/microservicios-spring-boot-confenalco/assets/31961588/77a86fc1-0700-4f48-a72c-f3ba000dd45c)

## 2. Enable eureka client

En la clase **GetwayApplication** se debe colocar la anotación **@EnableDiscoveryClient** para activar el cliente de Eureka de este proyecto. 

![image](https://github.com/crodrigr/microservicios-spring-boot-confenalco/assets/31961588/22485032-1c0e-4ef3-9e03-75212ec94f2b)

<br>
<br>
<br>

## 3. Configuración application.properties

En el configuración del **aplication.properties** se coloca el nombre del proyecto y el puerto. En esta caso el puerto de la puerta de enlace, es fijo, por que siempre va ser el mismo punto para ingresa al *ecosistema de microservicios**

![image](https://github.com/crodrigr/microservicios-spring-boot-confenalco/assets/31961588/ee6c967c-11e1-48d9-bfa8-258b635f8c73)

<br>
<br>
<br>

## 4. Confguración del application.yml

En el proyecto **getway de spring** la configuración se hace en otro tipo de archivo de configuración muy utilizado, que es el **yaml**. Este archivo es de tipo jerárquico.  

![image](https://github.com/crodrigr/microservicios-spring-boot-confenalco/assets/31961588/e1f15677-94a7-449e-bb80-7cc15cf138e1)

Ahora en el proyecto se tiene dos puertas de enlaces, **zuul** y **getway**, ambas están configuradas con el puerto **8090**. Solo se pueda usar una a la vez. 

<br>
<br>
<br>

## 5. Filtro global

Un filtro gobla aplica para toda petición que llege al **getway** tanto, pre y pos filtre. Se crea una clase **EjemploGlobalFilter** que implemete la interfaz **GlobalFilter**. En el **pre** agrega un toke en el header en el **request** y en el **post** obtiene el toke y en el header de response envia el token recuperado que se envio en el **pre**, una **cooke** color rojo y **conten-type** en text

### 5.1 EjemploGlobalFilter

![image](https://github.com/crodrigr/microservicios-spring-boot-confenalco/assets/31961588/95a61148-4932-4aea-a359-8098e691415e)

### 5.2 Respuesta 

![image](https://github.com/crodrigr/microservicios-spring-boot-confenalco/assets/31961588/2d5c172c-9952-41e6-830e-641390ff160f)


## 6. Filtro personalizado

En el directorio **factory** se crea una clase Ejemplo**GatewayFilterFactory**, la cual, va ser un filtro personalizado. Los filtros personlizados llevan siempre **GatewayFilterFactory** y lo precide el nombre del filtro. 

![image](https://github.com/crodrigr/microservicios-spring-boot-confenalco/assets/31961588/591824c3-6bf7-47f7-b919-06a73d510a49)

<details><summary>Mostrar código</summary>
<p>

```java {style="background-color: #ffffff;"}
package com.comfenalco.microservicio.getway.filters.factory;

import java.util.Arrays;
import java.util.List;
import java.util.Optional;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.cloud.gateway.filter.GatewayFilter;
import org.springframework.cloud.gateway.filter.factory.AbstractGatewayFilterFactory;
import org.springframework.http.ResponseCookie;
import org.springframework.stereotype.Component;

import reactor.core.publisher.Mono;

@Component
public class EjemploGatewayFilterFactory extends AbstractGatewayFilterFactory<EjemploGatewayFilterFactory.Configuracion>{

	private final Logger logger = LoggerFactory.getLogger(EjemploGatewayFilterFactory.class);
	
	
	public EjemploGatewayFilterFactory() {
		super(Configuracion.class);
	}

	@Override
	public GatewayFilter apply(Configuracion config) {
		return (exchange, chain) -> {
	
			logger.info("ejecutando pre gateway filter factory: " + config.mensaje);
			return chain.filter(exchange).then(Mono.fromRunnable(() -> {
				
				Optional.ofNullable(config.cookieValor).ifPresent(cookie -> {
					exchange.getResponse().addCookie(ResponseCookie.from(config.cookieNombre, cookie).build());
				});
				
				logger.info("ejecutando post gateway filter factory: " + config.mensaje);
				
			}));
		};
	}
	

	@Override
	public String name() {
		return "EjemploCookie";
	}

	@Override
	public List<String> shortcutFieldOrder() {
		return Arrays.asList("mensaje", "cookieNombre", "cookieValor");
	}

	public static class Configuracion {

		private String mensaje;
		private String cookieValor;
		private String cookieNombre;
		public String getMensaje() {
			return mensaje;
		}
		public void setMensaje(String mensaje) {
			this.mensaje = mensaje;
		}
		public String getCookieValor() {
			return cookieValor;
		}
		public void setCookieValor(String cookieValor) {
			this.cookieValor = cookieValor;
		}
		public String getCookieNombre() {
			return cookieNombre;
		}
		public void setCookieNombre(String cookieNombre) {
			this.cookieNombre = cookieNombre;
		}
		
		
	}

}



```
</p>
</details>

<br>
<br>
<br>

### 6.1 Configuración application.yml

Para que el filtro se aplique se debe configurar en el archivo de configuración **application.yml**. Este filtro se va aplicar cuando se invoca al proyecto **servicio-producto**. 

![image](https://github.com/crodrigr/microservicios-spring-boot-confenalco/assets/31961588/058261f4-b864-40a5-b50b-7c6291aeadad)


<br>

### 6.2 Repuesta

![image](https://github.com/crodrigr/microservicios-spring-boot-confenalco/assets/31961588/23424e65-d170-4005-a318-9a18e0abb9d8)


## 7. Filtros de fabrica

Son los filtros que ya viene definidos, como por ejemplo, **RequestHeader** y **ReponseHeader**. Se configura en **aplication.yml**. 


![image](https://github.com/crodrigr/microservicios-spring-boot-confenalco/assets/31961588/9df0e615-ad7a-4d68-963e-48af6bf497f3)

### 7.1 ItemController

En **ItemController** en el endpoint **/listar** se obtine los parametros enviado en el **pre-filter** por **@RequestParam** y **@RequestHeader**

![image](https://github.com/crodrigr/microservicios-spring-boot-confenalco/assets/31961588/0fd47c16-a2a5-4517-b5ea-0f0b554e3065)

**Response Header**

![image](https://github.com/crodrigr/microservicios-spring-boot-confenalco/assets/31961588/d5b7e44b-eaaf-41cc-b047-dfeedb4bf58f)

