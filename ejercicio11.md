/*11) En un supermercado hay una caja autónoma (sin un empleado que la atienda) que es usada 
por los clientes para pagar las compras realizadas. Hay N clientes que la deben usar de acuerdo
con el orden de llegada (suponga que existe una función UsarCaja() que simula este uso).
Nota: sólo se pueden usar los procesos que representan a los clientes.*/

sem mutex = 1;
bool Libre =true;
cola c;
sem espera[C]= ([C]0);

Process Cliente [id: 1..C]{
    int id;
    p(mutex); 
    if (Libre){
        Libre = false;
        v(mutex);
    } else {
        cola.push(id);
        v(mutex);
        p(espera[id]);
    }

    usarCaja();

    p(mutex)
    if(empty (c)){
        Libre = true;
    } else {
        cola.pop(id);
        v(espera[id]);
    }
    v(mutex);

}

