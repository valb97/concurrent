/*En una acopiadora de cereales hay
2 empleados, uno para atender a los camiones de maíz y
otro para los de girasol. Hay 30 camiones que llegan para descargar su carga (15 de maíz y
15 de girasol), cuando el camión llega espera hasta que el empleado correspondiente le avise
que puede descargar el cereal. Cada empleado hace descargar los camiones que le
corresponden de a uno a la vez y de acuerdo con orden de llegada. Nota: maximizar
concurrencia; el camión sabe qué tipo de cereal lleva; todos los procesos deben terminar*/


N = 30;



Process Camion [id:0..n-1]{
    int tipo;
    administrador[id].llegue(); 
    --el camion descarga
    administrador[id].salir();

}

Process Empleado[id:0..1]{
    for i:= 1 to 15 do {
            Administrador[id].siguiente(id);        
    } 
}

Monitor Gestion[id:0..1]{
    cond aviso;
    cond sig;
    cond salida;
    int esperando=0;

    procedure llegue(){
        esperando++;
        signal(aviso);
        wait(sig); // se duerme el camion
    }
    procedure salir(){
        signal(salida);
    }

    procedure siguiente(){
        if (esperando == 0){
            wait(aviso); //empleado esperando
        }
        esperando--;
        signal(sig);
        wait(salida);
    }
}
