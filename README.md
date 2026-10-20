# practica-sincronizacion
Durante esta práctica se observan distintos métodos de sincronzización.\
El fin es entender la necesidad de los mismo mediante un ejemplo simple el cual trata sobre la suma de un variable x.\
## Métodos utilizados
Los métodos a probar son:
* Lock
* Spin Lock
* Semáforo
* Barrera

## Compilación
```java
javac nombreArchivo.java
java nombreArchivo
```

## Desarrollo
### Lock
El uso básico del candado se encarga de bloquear a los hilos para que no puedan entrar a una sección crítica si existe otro hilo en la misma  sección.\
Se declara el candado:
```java
static final Lock CANDADO = ReentrantLock();

// Y para su manejo se utilizan los comandos lock y unlock

CANDADO.lock();
CANDADO.unlock();
```

### Spin Lock
Es similar al candado, con la diferencia de que al no lograr entrar al candado, en lugar de continuar con el programa, se queda ahí esperando el momento para poder entrar.
Se declara de la misma manera y cambia su uso:
```java
static final ReentrantLock CANDADO = ReentrantLock();

// Para intentar entrear utilizamos un while
while(!CANDADO.tryLock()){
    Thread.onSpinWait();
}

// Para cerrar
CANDADO.unlock();
```

### Semáforo
El semáforo permite sólo a cierca cantidad de hilos pasar por el bloque de código.\
Para este caso queremos solamente uno, lo que lo hace como un candado pero que necesita más recursos, es más efectivo en otros.
Se usa de la siguiente manera:
```java
Semaphore SEMAFORO = new Semaphore(N); // N siendo la cantidad de hilos que pueden pasar

// Intentan entrar con
SEMAFORO.acquire();

// Salen con
SEMAFORO.release();
```

### Barrera
La barrera detiene a todos los hilos y no los deja pasar hasta que tiene cierta cantidad específica de hilos esperando.
Funciona de la siguiente manera:
```java
CyclicBarrier BARRERA = new CyclicBarrier(
    HILOS, // Cantidad de hilos que quiere para dejar pasar
    () -> System.out.println(
        "Todos llegaron a la barrera. X = " + x)); // Mensaje que dará cuando todos lleguen a la barrera
```
Para utilizarse sólo usamos una función
```java
// Para utilizarse se hace lo siguiente:
BARRERA.await(); // Pone a los hilos a esperar a que se junten y 
```
En este caso se necesita un candado para limitar a cada uno de los hilos, al igual que en el semáforo, existen otros casos en los que puede ser más útil su uso