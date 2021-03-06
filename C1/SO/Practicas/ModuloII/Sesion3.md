## Ejercicio 1
Lo más destacable aquí es que el proceso padre se ejecuta antes que el hijo. Podéis comprobarlo. Por ejemplo, he aquí un log de la terminal:

```
Ahora soy el padre (22406), y el de mi hijo era 22407. Vamos a ver si el número es divisible por 4
4 es divisible por 4
Proceso con pid 22407, y parent 22406. Se comprobará si el número es par o impar
4 es par
```

Código del programa:
```c
#include<sys/types.h>
#include<unistd.h>
#include<stdio.h>
#include<errno.h>
#include <stdlib.h>

int main(int argc, char const *argv[]){
    if (argc != 2)
    {
    	printf("Número incorrecto de parámetros");
        printf("Parámetros: ./ejecutable número\n");
        exit(-1);
    }


    pid_t pid = fork();			//pid_t es el tipo de dato para los pid de los procesos
    int numero = atoi(argv[1]);		//atoi es una función que tranforma una cadena de caracteres en un entero

    if (pid < 0)
    {
        printf("No se ha podido crear un fork\n");
        exit(-1);
    }   
    else if (pid == 0)			// Proceso hijo
    {
        printf("Soy el proceso hijo con pid %d, y mi padre tiene pid: %d\n Se comprobará si el número es par o impar\n", getpid(), getppid());


        if (numero % 2 == 0)
            printf("%d es par\n", numero);
        else
            printf("%d es impar\n", numero);
    }
    else
    {
        printf("Soy el proceso padre %d, y mi proceso hijo tiene pid; %d.\n Vamos a ver si el número es divisible por 4\n", getppid(), getpid());

        if (numero % 4 == 0)
            printf("%d es divisible por 4\n", numero);
        else
            printf("%d no es divisible por 4\n", numero);
    }

    exit(1);
}
```

## Ejercicio 2
```c
#include<sys/types.h>
#include<unistd.h>		
#include<stdio.h>
#include<errno.h>
#include <stdlib.h>

int global=6;
char buf[]="cualquier mensaje de salida\n";

int main(int argc, char *argv[]) {
	int var;
	pid_t pid;

	var=88;

	if(write(STDOUT_FILENO,buf,sizeof(buf)+1) != sizeof(buf)+1) {
		perror("\nError en write");
		exit(EXIT_FAILURE);
	}

	//(1)if(setvbuf(stdout,NULL,_IONBF,0)) {
	//	perror("\nError en setvbuf");
	//}

	printf("\nMensaje previo a la ejecución de fork");

	if( (pid=fork())<0){
		perror("\nError en el fork");
		exit(EXIT_FAILURE);
	}
	else if(pid==0){  
		//proceso hijo ejecutando el programa
		global++;
		var++;
	} else  //proceso padre ejecutando el programa
		sleep(1);

	printf("\npid= %d, global= %d, var= %d\n", getpid(),global,var);
	exit(EXIT_SUCCESS);
}
```

`Write()` lo que hace es escribir la cadena de caracteres en la salida estándar
A continuación, crea el proceso hijo con `fork()`. Si no lo consigue alocar, se termina el programa
- Si el pid vale 0, se ejecuta el proceso hijo. Este incrementa dos variables
- Si no, ejecuta el código del padre. Este lo único que hace es esperarse un segundo

Descomentar las líneas de código adyacentes a (1) no tienen ningún efecto en el valor de las variables. Lo único que hace es deshabilitar el buffer de printf a salida estándar?

Los valores de las variables mostrados son diferentes porque son contextos diferentes. Al hacer un fork, se hace una copia de la memoria. En principio, hace falta especificarle que sea capaz de cambiar los valores del padre

Salida con buffer:
```
Mensaje previo a la ejecución de fork
pid= 3154, global= 7, var= 89
Mensaje previo a la ejecución de fork
pid= 3153, global= 6, var= 88
```
Salida sin buffer:
```

Mensaje previo a la ejecución de fork
pid= 3346, global= 7, var= 89

pid= 3345, global= 6, var= 88
```
## Ejercicio 3
> Indica qué tipo de jerarquías de procesos se generan mediante la ejecución de cada uno de los siguientes fragmentos de código. Comprueba tu solución implementando un código para generar 20 procesos en cada caso, en donde cada proceso imprima su PID y el del padre, PPID.
Lo que haremos será crear 20 procesos hijos distintos, todos anidados:
```
           Padre              Padre              Padre
Proceso 1 -------> Proceso 2 -------> Proceso 3 -------> Proceso 4 ...
```
Podemos comprobarlo con el output:
```
Identificador del proceso 7707, identificador del proceso padre: 7706,
Identificador del proceso 7706, identificador del proceso padre: 7705,
Identificador del proceso 7705, identificador del proceso padre: 7704,
Identificador del proceso 7704, identificador del proceso padre: 7703,
Identificador del proceso 7703, identificador del proceso padre: 7702,
Identificador del proceso 7702, identificador del proceso padre: 7701,
Identificador del proceso 7701, identificador del proceso padre: 7700,
Identificador del proceso 7700, identificador del proceso padre: 7699,
Identificador del proceso 7699, identificador del proceso padre: 7698,
Identificador del proceso 7698, identificador del proceso padre: 7697,
Identificador del proceso 7697, identificador del proceso padre: 7696,
Identificador del proceso 7696, identificador del proceso padre: 7695,
Identificador del proceso 7695, identificador del proceso padre: 7694,
Identificador del proceso 7694, identificador del proceso padre: 7693,
Identificador del proceso 7693, identificador del proceso padre: 7692,
Identificador del proceso 7692, identificador del proceso padre: 7691,
Identificador del proceso 7691, identificador del proceso padre: 7690,
Identificador del proceso 7690, identificador del proceso padre: 7689,
Identificador del proceso 7689, identificador del proceso padre: 7688,
Identificador del proceso 7688, identificador del proceso padre: 4436,
```
Código de nuestro programa:

```c
#include <stdio.h>
#include <stdlib.h> //poder utilizar exit
#include <sys/types.h> //pid_t tipy
#include <errno.h>
#include <unistd.h> //fork, pipe , write
#include <sys/wait.h>

int main( int arg , char * argv[])
{
  pid_t id_proceso;
  pid_t id_padre;
  pid_t pid = 0;
  int i = 0;
  while( i++ < 20 && pid == 0) 
    {
      //creamos el hijillo
	  if( (pid=fork())  < 0)
	  {
		  perror("\n Por algún motivo que ahora mismo desconozco, no se ha podido crear el hijo\n");
		  exit(-1);
	  }
      
    }
  id_proceso = getpid();
  id_padre = getppid();
  while(wait(NULL) != -1);
  printf (" \nIdentificador del proceso %d, identificador del proceso padre: %d,\n" , id_proceso , id_padre );
  exit (0);
}

```
Usamos `while(wait(NULL) != -1)` para esperar a que todos los procesos acaben debidamente. Hemos de destacar que wait solo se preocupa de los hijos más próximos. Le da igual los nietos


Por tanto, estamos ante la jerarquía tipo 1 del programa dado: 

```c
#include<sys/types.h>
#include<sys/wait.h>
#include<unistd.h>
#include<stdio.h>
#include<errno.h>
#include <stdlib.h>

int main(){
    int num_procesos = 20;
    pid_t childpid;

    // Jerarquía tipo 1 //
    for (int i=1; i < num_procesos; i++){
        if ( (childpid = fork()) == -1){
            fprintf(stderr, "Could not create child %d: %s\n",i,strerror(errno));
            exit(-1);
        }


        if (childpid){
            printf("Padre: %d, proceso: %d\n", getppid(), getpid());
            break;
        }
    }

    // Jerarquía tipo 2 //
    for (int i=1; i < num_procesos; i++){
        if ( (childpid = fork()) == -1){
            fprintf(stderr, "Could not create child %d: %s\n",i,strerror(errno));
            exit(-1);
        }


        if (!childpid){
            printf("Padre: %d, proceso: %d\n", getppid(), getpid());
            break;
        }
    }
}
```

## Ejercicio 4

Es posible que este ejercicio esté mal. Necesita revisió
Código del programa:

```c
#include<sys/types.h>
#include<unistd.h>
#include<stdio.h>
#include<errno.h>
#include <stdlib.h>

int main(){
    int i, estado;
    pid_t PID;

    // printf() as soon as posible
    setvbuf(stdout, (char*)NULL, _IONBF, 0);

    // Creación de hijos
    for(i=0; i<5; i++){
         if ((PID = fork()) <0){
            perror("Error en fork \n");
            exit(-1);
        }

        if (PID==0){  //Hijo imprime y muere
            printf("Soy el hijo PID = %i\n", getpid());
            exit(0);
        }
    }

    // Esperamos hijos
    for (i=4; i>=0; i--){
        PID = wait(&estado);
        printf("Ha finalizado el hijo con PID = %i\n", PID);
        printf("Solo me quedan %i hijos vivos\n\n", i);
    }

    exit(0);
}
```
Cuesta ver la lógica del programa, así que analicémosla:
- Los procesos hijos se empiezan a crear. Imprimen una cadena, y se cierran
- Mientras tanto, el segundo bloque for() los vigila. Es decir, `PID = wait()` vigila constantemente el cambio en el estado del proceso. Si alguno termina, entonces, el bucle continúa.

Podemos ver el comportamiento del programa mejor si quitamos el buffer de `printf()`:
```
Soy el hijo PID = 22199
Soy el hijo PID = 22200
Ha finalizado el hijo con PID = 22199
Solo me quedan 4 hijos vivos

Soy el hijo PID = 22201
Ha finalizado el hijo con PID = 22200
Solo me quedan 3 hijos vivos

Soy el hijo PID = 22202
Soy el hijo PID = 22203
Ha finalizado el hijo con PID = 22201
Solo me quedan 2 hijos vivos

Ha finalizado el hijo con PID = 22202
Solo me quedan 1 hijos vivos

Ha finalizado el hijo con PID = 22203
Solo me quedan 0 hijos vivos
```
¿Podéis observar cómo se entremezclan ambos bucles?

## Ejercicio 5
Modificación muy sencilla del anterior:
```c
#include<sys/types.h>
#include<unistd.h>
#include<stdio.h>
#include<errno.h>
#include <stdlib.h>

int main(){
    int i, estado;
    int hijos_vivos=5;
    pid_t PIDs[5];

    // printf() as soon as posible
    setvbuf(stdout, (char*)NULL, _IONBF, 0);

    // Creación de hijos
    for(i=0; i<5; i++){
        if ((PIDs[i] = fork()) <0){
            perror("Error en fork \n");
            exit(-1);
        }

        if (PIDs[i]==0){  //Hijo imprime y muere
            printf("Soy el hijo PID = %i\n", getpid());
            exit(0);
        }
    }

    // Padre que espera a los pares
    for (i=4; i>=0; i -= 2){
        waitpid(PIDs[i], &estado);

        printf("Ha finalizado el hijo con PID = %i y estado %d\n", PIDs[i], estado);
        printf("Solo me quedan %d hijos vivos\n\n", --hijos_vivos);
    }

    // Padre que espera a los impares
    for (i=3; i>=1; i -= 2){
        waitpid(PIDs[i], &estado);

        printf("Ha finalizado el hijo con PID = %i y estado %d\n", PIDs[i], estado);
        printf("Solo me quedan %d hijos vivos\n\n", --hijos_vivos);
    }

    // Sleep(2) para garantizar que han finalizado las operaciones de salida    
    sleep(2);
    printf("Ya no me quedan hijos :(");

    exit(0);
}
```

Salida de la terminal:
```txt
Soy el hijo PID = 6361
Soy el hijo PID = 6362Soy el hijo PID = 6363
Ha finalizado el hijo con PID = 6364 y estado 32593
Solo me quedan 4 hijos vivos

Ha finalizado el hijo con PID = 6362 y estado 32593
Solo me quedan 3 hijos vivos

Ha finalizado el hijo con PID = 6360 y estado 32593
Solo me quedan 2 hijos vivos

Ha finalizado el hijo con PID = 6363 y estado 32593
Solo me quedan 1 hijos vivos

Ha finalizado el hijo con PID = 6361 y estado 32593
Solo me quedan 0 hijos vivos

Soy el hijo PID = 6364
Soy el hijo PID = 6360
Ya no me quedan hijos :(
```

## Ejercicio 6
Código del programa:
```c
#include<sys/types.h>
#include<sys/wait.h>
#include<unistd.h>
#include<stdio.h>
#include<errno.h>
#include <stdlib.h>

int main(int argc, char *argv[]){
	pid_t pid;
	int estado;

	if( (pid=fork())<0) {
		perror("\nError en el fork");
		exit(EXIT_FAILURE);
	}
	else if(pid==0) {  //proceso hijo ejecutando el programa
		if(execl("/usr/bin/ldd", "ldd", "./tarea5") < 0) {
			perror("\nError en el execl");
			exit(EXIT_FAILURE);
		}
	}

	wait(&estado);
	/*
	<estado> mantiene información codificada a nivel de bit sobre el motivo de finalización del proceso hijo
	que puede ser el número de señal o 0 si alcanzó su finalización normalmente.
	Mediante la variable estado de wait(), el proceso padre recupera el valor especificado por el proceso hijo como argumento de la llamada exit(), pero desplazado 1 byte porque el sistema incluye en el byte menos significativo
	el código de la señal que puede estar asociada a la terminación del hijo. Por eso se utiliza estado>>8
	de forma que obtenemos el valor del argumento del exit() del hijo.
	*/

	printf("\nMi hijo %d ha finalizado con el estado %d\n",pid, estado>>8);

	exit(EXIT_SUCCESS);
}
```
Primero, alojamos un proceso nuevo con `fork()`, como vimos en los ejercicios anteriores. Si hemos conseguido crearlo, ejecutaremos el programa `ldd` con la función `execl()` dentro de C. Para ello, indicamos la ruta donde se encuentra el progama, con la orden y el nombre del ejecutable de nuestro programa. Si no conseguimos que se ejecute, nuestro programa en C se cerrará.
Mientras tanto, `wait()` le echa un ojo a lo que está ocurriendo en el proceso hijo mediante la variable estado. Está bien documentada en el código.

## Ejercicio 7
Código del programa:
```c
#include<sys/types.h>
#include<unistd.h>
#include<stdio.h>
#include<errno.h>
#include<stdlib.h>
#include<string.h>
#include<stdbool.h>
#include <malloc.h> //para reservar memoria



int main(int argc, char *argv[])
{

  //Compruebo el número de argumentos
	if(argc==1){
		printf("Número de argumentos incorrecto (%d).\n", argc);
		exit(-1);
	}

  //Copio los argumentos del programa a ejecutar
	char * argumentos[argc];
	bool bg = false; //foreground o background
	int tam;

	int contador = 0;
	for(int i=1; i<argc; i++){
		if(strcmp(argv[i], "bg")==0){
			bg = true;
		}
		else{
			argumentos[contador] = (char *) malloc(strlen(argv[i])+1);
			strcpy(argumentos[contador], argv[i]);
			argumentos[contador][strlen(argv[i])];
			contador++;
		}
	}
	tam = contador;
	argumentos[contador] = NULL; /*el array que se pasa como argumento a execvp debe tener NULL en su última componente*/


  //Ajusto los argumentos a pasar a execvp
	char ruta[strlen(argumentos[0])+1];
	strcpy(ruta, argumentos[0]); //primer argumento del execvp

  /*La primera cadena del array pasado a execvp solo debe tener el nombre del programa a ejecutar, no toda la ruta*/
	int posicion = -1;
	for(int i=strlen(ruta) && posicion!=-1; i>=0; i--){
		if(ruta[i] == '/')
			posicion = i;
	}

	strcpy(argumentos[0], "");
	contador = 0;
	for(int i = posicion+1; i<strlen(ruta); i++){
		argumentos[0][contador] = ruta[posicion];
		contador++;
	}

  //Ejecutamos el programa
	if(bg){
		pid_t PID;
		if((PID = fork())<0){
			perror("Error en el fork.\n");
			exit(-1);
		}

		if(!PID){
			if(execvp(ruta, argumentos)==-1){
				perror("Error en execvp.\n");
				exit(-1);
			}
		}
	}

	else{
		if(execvp(ruta, argumentos)==-1){
				perror("Error en execvp.\n");
				exit(-1);
		}
	}
	
  //Libero la memoria
  	for(int i=0; i<tam; i++){
		free(argumentos[i]);
	}
	
	exit(EXIT_SUCCESS);
}

/*Al ejecutarlo con valgrind da el siguiente error de lectura
(aunque el programa sigue ejecutándose correctamente):

Invalid read of size 1
at 0x80487BD: main
Address 0xbe9a9f0f is on thread 1's stack
1 bytes below stack pointer

*/

```
