# Videopic
Un sencillo circuito con un pic 16f877 realizado en ASM, que es capaz de mostrar en una pantalla de televisión carácteres gráficos en blanco y negro.

Dispone de dos modos de video, el modo de video 0 (20x8 carácteres) y el modo de video 1 (7x11 carácteres), seleccionables a traves de la etiqueta MODO_PANT del programa ensamblador. Si el valor de esta etiqueta es 0, el modo de video presentado es cero, si vale uno el modo de video será uno.
Los textos presentados en la pantalla se pueden cambiar por otros,modificando los carácteres que hay en las etiquetas TEXTO y TEXTO2, estos han de ser obligatoriamente carácteres de la ''A' a la 'Z' escritos en minúsculas, aunque en la pantalla se presentan en mayúsculas.
La tensión de entrada que se  tiene que suministrar al circuito para que funcione ha de ser de unos 9V hasta más o menos unos 18V, por la potencia que consume el circuito cualquier transformador universal multitensión hará que este funcione.
