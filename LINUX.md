# Linux
==========================
* Para encontrar que procesos utilizan que puertos
  > netstat -tulpn | grep :80 
* Para terminar/matar el proceso que hace uso de un determinado puerto
  > fuser -k 8080/tcp 
