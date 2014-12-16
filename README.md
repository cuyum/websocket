Websocket
=========

POC de Websocket tomcat7 

Requisitos:
---
 * tomcat 7+
 * maven 3+
 * jdk 7+
 * apache 2+

Instrucciones
--- 
1. Descomprimir tomcat e iniciarlo (automáticamente en http://localhost:8080)
2. Compilar el proyecto con mvn clean package y copiar el war ubicado en target/ al directorio webapps de tomcat
3. Activar módulos de apache mod_proxy mod_proxy_html mod_proxy_wstunnel
https://wiki.apache.org/httpd/TomcatReverseProxy
http://httpd.apache.org/docs/2.4/mod/mod_proxy.html
http://httpd.apache.org/docs/2.4/mod/mod_proxy_wstunnel.html

4. Editar el archivo de configuración del host por defecto "localhost" (agregar lo siguiente)

	```
	<VirtualHost>
	...
	    # mod_proxy setup.
	    ProxyRequests Off
	    ProxyPass /websockets http://localhost:8080/websockets
	    ProxyPassReverse /websockets http://localhost:8080/websockets
	
	    <Location "/websockets">
	  	  # Configurations specific to this location. Add what you need.
		  # For instance, you can add mod_proxy_html directives to fix
	      # links in the HTML code. See link at end of this page about using
	      # mod_proxy_html.
	
	      # Allow access to this proxied URL location for everyone.
		  Order allow,deny
	      Allow from all
	    </Location>
	
		ProxyPass /socket/  ws://localhost:8080/websockets/
		ProxyPass /ssocket/ wss://localhost:8080/websockets/
	.....
	</VirtualHost>
	```
5. Acceder a http://localhost:8080/websockets o http://localhost/websockets
6. Elegir la conexión que se quiere establecer del websocket (Directa al tomcat o Proxy por apache)
7. Apretar el botón "Conect"
8. Transmitir al server un mensaje que luego el server transmitirá de vuelta a la página

