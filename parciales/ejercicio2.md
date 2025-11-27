/*Existen
B barcos areneros que deben descargar su contenido en una playa. Los barcos se
descargan de a uno por vez, y de acuerdo con orden de llegada. Una vez que el barco llegó,
espera a que le llegue su turno para comenzar a descargar su contenido, y luego se retira.
Nota: sólo se pueden usar los procesos que representen a los barcos; cada barco descarga
sólo una vez; suponga que existe la función DESCARGAR() que simula que el barco está
descargando su contenido en la playa*/

sem mutex = 1
cola c;
sem Espera[B]=([B]0);
bool Libre = true;


Process Barcos [id:1..B]{

    p(mutex);
    if (Libre){
        Libre=false
        v(mutex);

    } else {
        push(c,id);
        v(mutex);
        p(espera[id]);
    }

    DESCARGAR();

    p(mutex);
    if(empty (C)) {
        Libre=true;
    } else {
        pop(c,id);
        v(Espera[id]);
    }
    v(mutex);

}