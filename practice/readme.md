
ALGORITMO

1.Recibir cadena

2.Verificar longitud <= 20

3.Eliminar espacios en blanco

4.Verificar que los caracteres sean correctos

5.Declarar funcion recursiva con las operaciones a realizar

6.Separar la cadena en dos arreglos uno de numeros y otra de operadores

7.Primero resolver raices, exponentes,llamando a la función de operaciones mandando los atributos necesarios.
  Actualizar valores de los arreglos
  
8.Segundo resolver multiplicaciones y divisiones, llamar a la función recursiva.
  Actualizar valores de los arreglos
  
9.Tercero realizar sumas y restas, llamar a la función recursiva.
  Actualizar valores de los arreglos.
  
10. Obtener el resultado.

SOLUCIÓN    

    //Funcion para calculo el valor de la cadena ingresada
    function calculo(expresionIngresada) {
    //Funcion para dividir numeros y operadores en diferentes arreglos
    function dividirexpresionIngresada(expresionIngresada) {
        const arregloNumeros = expresionIngresada.split(/[\+\-\*\/^]|r/).filter(Boolean);;
        const arregloOperadores = expresionIngresada.split(/[\d.]+/).join('').split('');
        return { arregloNumeros, arregloOperadores };
    }
    let { arregloNumeros, arregloOperadores } = dividirexpresionIngresada(expresionIngresada);
    //console.log("Números:", arregloNumeros);
    //console.log("arregloOperadores:", arregloOperadores);
    //declaro varaiable para recorrer los arreglos
    let i = 0;


    //Funcion recursiva, que me permite llamar a la operación segun sea el caso.
    function operar(operador, operando1, operando2) {
        switch (operador) {
            case '*':
                return operando1 * operando2;
            case '/':
                return operando1 / operando2;
            case '+':
                return operando1 + operando2;
            case '-':
                return operando1 - operando2;
            case 'r':
                return Math.sqrt(operando1);
            case '^':
                return Math.pow(operando1, operando2);
            default:
                throw new Error('Operador inválido: ' + operador);
        }
    }

    // Ciclo para recorrer los arreglos de numeros y letras , buscando primero las raices y exponentes
    while (i < arregloOperadores.length) {
        //si es raiz , unicamente necesito el numero en posicion i
        if (arregloOperadores[i] === 'r') {
            const operando1 = parseFloat(arregloNumeros[i]);
            const resultado = operar('r', operando1, 0);
            //actualizo el arreglo de los numeros con el nuevo valor, y elimino el operador usado
            arregloNumeros.splice(i, 1, resultado);
            arregloOperadores.splice(i, 1);
        }
        //si es exponente, necesito el numero que es base y el numero que es exponente
        else if (arregloOperadores[i] === '^') {
            const operando1 = parseFloat(arregloNumeros[i]);
            const operando2 = parseFloat(arregloNumeros[i + 1]);
            const resultado = operar('^', operando1, operando2);
            //actualizo el arreglo de los numeros con el nuevo valor, y elimino el operador usado
            arregloNumeros.splice(i, 2, resultado);
            arregloOperadores.splice(i, 1);
        } else {
            i++;
        }
    }
    // Ciclo para recorrer los arreglos de numeros y letras , buscando de segundo multiplicaciones y divisiones
    //VUelvo a 0 mi variable para recorrer nuevamente los arreglos
    i = 0;
    while (i < arregloOperadores.length) {
        if (arregloOperadores[i] === '*' || arregloOperadores[i] === '/') {
            const operando1 = parseFloat(arregloNumeros[i]);
            const operando2 = parseFloat(arregloNumeros[i + 1]);
            const resultado = operar(arregloOperadores[i], operando1, operando2);
            //actualizo el arreglo de los numeros con el nuevo valor, y elimino el operador usado
            arregloNumeros.splice(i, 2, resultado);
            arregloOperadores.splice(i, 1);
        } else {
            i++;
        }
    }

    // Ciclo para recorrer los arreglos de numeros y letras , buscando por ultimo sumas y restas
    // vUelvo a 0 mi variable para recorrer nuevamente los arreglos
    i = 0;
    while (i < arregloOperadores.length) {
        const operando1 = parseFloat(arregloNumeros[i]);
        const operando2 = parseFloat(arregloNumeros[i + 1]);
        const resultado = operar(arregloOperadores[i], operando1, operando2);
        //actualizo el arreglo de los numeros con el nuevo valor, y elimino el operador usado
        arregloNumeros.splice(i, 2, resultado);
        arregloOperadores.splice(i, 1);
    }

    //por ultimo parseo mi respuesta en flotante 
    return parseFloat(arregloNumeros[0]);
  }



    //Función para verificar que la cadena tenga caracteres válidos
    function verificarCaracteres(expresionIngresada) {
    var expresionIngresadaCorrecta = /[^0-9\+\-\*\/\(\)\^r]/;
    for (var i = 0; i < expresionIngresada.length; i++) {
        if (expresionIngresadaCorrecta.test(expresionIngresada.charAt(i))) {
            return false;
        }
    }
    return true;
    }

    function validaciones(expresionIngresada) {
    //Verifico que la expresión no exceda de 20 caracteres
    if (expresionIngresada.length <= 20) {
        //console.log("cadena correcta");
        //Elimino espacios en blanco en caso tenga alguno
        let expresionIngresadaSinEspacios = expresionIngresada.replace(/\s/g, '');
        //console.log(expresionIngresadaSinEspacios);
        //Valido que la expresion tenga numeros, +, -, *, /, r (raiz), ^(potencia)
        var validacion = verificarCaracteres(expresionIngresadaSinEspacios);
        if (validacion) {
            console.log("Expresion Ingresada válida.");
            return calculo(expresionIngresadaSinEspacios);
        } else {
            console.log("Expresion Ingresada inválida.");
        }
    } else {
        console.log("La cadena excede los 20 caracteres.");
    }
    }   

    //PRUEBAS
    const expresionIngresada3 = "4-7+ 8+9/2*3+5-6+8*95/";
    console.log(expresionIngresada3);
    console.log("El resultado es:", validaciones(expresionIngresada3));
    console.log("\n");

    const expresionIngresada1 = "4-7+ 8+9/2*3";
    console.log(expresionIngresada1);
    console.log("El resultado es:", validaciones(expresionIngresada1));
    console.log("\n");


    const expresionIngresada2 = "4^3";
    console.log(expresionIngresada2);
    console.log("El resultado es:", validaciones(expresionIngresada2));
    console.log("\n");

    const expresionIngresada = "4-7+8+9+r2+3+4";
    console.log(expresionIngresada);
    console.log("El resultado es:", validaciones(expresionIngresada));



RESULTADOS

![image](https://github.com/Internship-ciancoders-24-1/mi-lugar-de-trabajo-introduccion-ESMERALD22/assets/116524002/b078cc51-06c0-4ec2-a2c1-ed660f4590cd)



