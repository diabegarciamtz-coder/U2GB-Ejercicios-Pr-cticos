# U2GB-Ejercicios-Practicos
Ejercicios practicos

# Actividad 01: Manipulación de Lista Enlazada 
```
package ejPracticos;

/**
 *
 * @author Diana Mabel Garcia Martinez
 *          1224100672
 *          diabegarciamtz@gmail.com    18/10/25
 */
// Clase genérica para representar un nodo de la lista enlazada
public class Nodo<T> {
    public T dato;
    public Nodo<T> siguiente;
    
    // Constructor
    public Nodo(T dato) {
        this.dato = dato;
        this.siguiente = null;
    }

    public T getDato() {
        return dato;
    }

    public Nodo<T> getSiguiente() {
        return siguiente;
    }

    public void setDato(T dato) {
        this.dato = dato;
    }

    public void setSiguiente(Nodo<T> siguiente) {
        this.siguiente = siguiente;
    }
    
}
```
```
package ejPracticos;

/**
 *
 * @author Diana Mabel Garcia Martinez
 *          1224100672
 *          diabegarciamtz@gmail.com    18/10/25
 */
// Clase genérica para manejar la lista enlazada

public class ListaEnlazada<T extends Comparable<T>> {
    private Nodo<T> cabeza;
    private Nodo<T> cola;

    public ListaEnlazada() {
        cabeza = null;
        cola = null;
    }

    public void insertarAlFinal(T dato) {
        Nodo<T> nuevo = new Nodo<>(dato);
        if (cabeza == null) {
            cabeza = nuevo;
            cola = nuevo;
        } else {
            cola.setSiguiente(nuevo);
            cola = nuevo;
        }
    }

    public void mostrarLista() {
        if (cabeza == null) {
            System.out.println("La lista está vacía.");
            return;
        }
        Nodo<T> actual = cabeza;
        System.out.print("Lista: ");
        while (actual != null) {
            System.out.print(actual.getDato() + " ");
            actual = actual.getSiguiente();
        }
        System.out.println();
    }

    public void eliminarMayoresQue(T limite) {
        while (cabeza != null && cabeza.getDato().compareTo(limite) > 0) {
            cabeza = cabeza.getSiguiente();
        }

        if (cabeza == null) {
            cola = null;
            return;
        }

        Nodo<T> actual = cabeza;
        while (actual.getSiguiente() != null) {
            if (actual.getSiguiente().getDato().compareTo(limite) > 0) {
                actual.setSiguiente(actual.getSiguiente().getSiguiente());
                if (actual.getSiguiente() == null) {
                    cola = actual;
                }
            } else {
                actual = actual.getSiguiente();
            }
        }
    }
}
```

```
package ejPracticos;

/**
 *
 * @author Diana Mabel Garcia Martinez
 *          1224100672
 *          diabegarciamtz@gmail.com    18/10/25
 */
import java.util.Random;
import java.util.Scanner;

public class Actividad01 {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Random random = new Random();
        ListaEnlazada<Integer> lista = new ListaEnlazada<>();
        
        // Generar 10 números aleatorios entre 1 y 100
        System.out.println("Generando números aleatorios...");
        for (int i = 0; i < 10; i++) {
            int numero = random.nextInt(100) + 1;
            lista.insertarAlFinal(numero);
        }
        
        // Mostrar la lista original
        lista.mostrarLista();
        
        // Solicitar valor límite al usuario
        System.out.print("Ingrese el valor límite para eliminar nodos mayores: ");
        int valorLimite = scanner.nextInt();
        
        // Eliminar nodos mayores al valor límite
        lista.eliminarMayoresQue(valorLimite);
        
        // Mostrar la lista después de la eliminación
        System.out.println("Lista después de eliminar nodos mayores a " + valorLimite + ":");
        lista.mostrarLista();
        
        scanner.close();
    }
}
```
# Actividad 02: Lista Enlazada de Palabras desde Archivo 
```
package ejPracticos;

import java.io.*;
import java.util.List;


/**
 *
 * @author Diana Mabel Garcia Martinez
 *          1224100672
 *          diabegarciamtz@gmail.com    18/10/25
 */
public class ManejandoArchivos<T> {

    // Guarda el contenido de una lista en un archivo de texto
    public void guardarEnArchivo(ListaEnlazada<T> lista, String nombreArchivo) {
        List<T> elementos = lista.toList(); // Requiere toList() en ListaEnlazada
        try (PrintWriter writer = new PrintWriter(new FileWriter(nombreArchivo))) {
            for (T elem : elementos) {
                writer.println(elem);
            }
            System.out.println("Datos guardados en: " + nombreArchivo);
        } catch (IOException e) {
            System.out.println("Error al guardar: " + e.getMessage());
        }
    }




    // Lee un archivo de texto y devuelve su contenido como cadena
    public String leerArchivo(String nombreArchivo) {
        StringBuilder contenido = new StringBuilder();
        try (BufferedReader reader = new BufferedReader(new FileReader(nombreArchivo))) {
            String linea;
            while ((linea = reader.readLine()) != null) {
                contenido.append(linea).append("\n");
            }
            System.out.println("Archivo leído correctamente.");
        } catch (IOException e) {
            System.out.println("No se pudo leer el archivo. Se creará uno nuevo.");
        }
        return contenido.toString();
    }
}

```
```

package ejPracticos;

/**
 *
 * @author Diana Mabel Garcia Martinez
 *          1224100672
 *          diabegarciamtz@gmail.com    18/10/25
 */
import java.util.Scanner;

public class Actividad02 {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        ListaEnlazada<String> lista = new ListaEnlazada<>();
        ManejandoArchivos<String> manejadorArchivos = new ManejandoArchivos<>();
        String nombreArchivo = "palabras.txt";
        
        // Leer archivo si existe y procesar palabras
        String contenido = manejadorArchivos.leerArchivo(nombreArchivo);
        if (!contenido.isEmpty()) {
            String[] palabras = contenido.split("\\s+");
            for (String palabra : palabras) {
                if (!palabra.trim().isEmpty()) {
                    lista.insertarAlFinal(palabra.trim());
                }
            }
        }
        
        // Mostrar palabras actuales
        lista.mostrarLista();
        
        // Menú de opciones
        int opcion;
        do {
            System.out.println("\n--- MENÚ ---");
            System.out.println("1. Agregar palabra");
            System.out.println("2. Eliminar palabra");
            System.out.println("3. Mostrar palabras");
            System.out.println("4. Guardar y salir");
            System.out.print("Seleccione una opción: ");
            opcion = scanner.nextInt();
            scanner.nextLine(); // Limpiar buffer
            
            switch (opcion) {
                case 1:
                    System.out.print("Ingrese la palabra a agregar: ");
                    String nuevaPalabra = scanner.nextLine();
                    lista.insertarAlFinal(nuevaPalabra);
                    break;
                    
                case 2:
                    System.out.print("Ingrese la palabra a eliminar: ");
                    String palabraEliminar = scanner.nextLine();
                    // Para eliminar necesitaríamos implementar un método de búsqueda
                    System.out.println("Funcionalidad de eliminación requeriría método adicional");
                    break;
                    
                case 3:
                    lista.mostrarLista();
                    break;
                    
                case 4:
                    manejadorArchivos.guardarEnArchivo(lista, nombreArchivo);
                    System.out.println("Saliendo...");
                    break;
                    
                default:
                    System.out.println("Opción no válida.");
            }
        } while (opcion != 4);
        
        scanner.close();
    }
}

```
# Actividad 03: Representación y Evaluación de Polinomios con Listas Enlazadas
```
package ejPracticos;

/**
 *
 * @author Diana Mabel Garcia Martinez
 *          1224100672
 *          diabegarciamtz@gmail.com    18/10/25
 */
public class Termino <C extends Number, E extends Number>{
    public C coeficiente;
    public E exponente;
    public Termino <C, E> siguiente;
    
    public Termino(C coeficiente, E exponente) {
        this.coeficiente = coeficiente;
        this.exponente = exponente;
        this.siguiente = null;
    }
}

```
```
package ejPracticos;

/**
 *
 * @author Diana Mabel Garcia Martinez
 *          1224100672
 *          diabegarciamtz@gmail.com    18/10/25
 */
// Clase genérica para lista enlazada de polinomios
public class Polinomio<C extends Number, E extends Number> {
    private Termino<C, E> cabeza;
    
    public Polinomio() {
        this.cabeza = null;
    }
    
    // Agregar término al polinomio
    public void agregarTermino(C coeficiente, E exponente) {
        Termino<C, E> nuevoTermino = new Termino<>(coeficiente, exponente);
        
        if (cabeza == null) {
            cabeza = nuevoTermino;
        } else {
            Termino<C, E> actual = cabeza;
            while (actual.siguiente != null) {
                actual = actual.siguiente;
            }
            actual.siguiente = nuevoTermino;
        }
    }
    
    // Evaluar el polinomio para un valor de x dado
    public double evaluar(double x) {
        double resultado = 0.0;
        Termino<C, E> actual = cabeza;
        
        while (actual != null) {
            double coef = actual.coeficiente.doubleValue();
            double exp = actual.exponente.doubleValue();
            resultado += coef * Math.pow(x, exp);
            actual = actual.siguiente;
        }
        
        return resultado;
    }
    
    // Mostrar el polinomio
    public void mostrarPolinomio() {
        Termino<C, E> actual = cabeza;
        boolean primerTermino = true;
        
        System.out.print("P(x) = ");
        while (actual != null) {
            double coef = actual.coeficiente.doubleValue();
            int exp = actual.exponente.intValue();
            
            if (coef != 0) {
                if (!primerTermino && coef > 0) {
                    System.out.print(" + ");
                } else if (coef < 0) {
                    System.out.print(" - ");
                }
                
                double coefAbs = Math.abs(coef);
                
                if (exp == 0) {
                    System.out.print(coefAbs);
                } else if (exp == 1) {
                    System.out.print(coefAbs + "x");
                } else {
                    System.out.print(coefAbs + "x^" + exp);
                }
                
                primerTermino = false;
            }
            actual = actual.siguiente;
        }
        System.out.println();
    }
}

```
```
package ejPracticos;

/**
 *
 * @author Diana Mabel Garcia Martinez
 *          1224100672
 *          diabegarciamtz@gmail.com    18/10/25
 */
import java.util.Scanner;

public class Actividad03 {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Polinomio<Double, Integer> polinomio = new Polinomio<>();
        
        System.out.println("INGRESO DE TÉRMINOS DEL POLINOMIO");
        System.out.println("Ingrese los términos en formato: coeficiente,exponente");
        System.out.println("Ejemplo: para 3x^4 - 4x^2 + 11 ingrese: 3,4 -4,2 11,0");
        System.out.println("Ingrese 'fin' para terminar");
        
        // Leer términos del polinomio
        while (true) {
            System.out.print("Ingrese término (coeficiente,exponente): ");
            String entrada = scanner.nextLine().trim();
            
            if (entrada.equalsIgnoreCase("fin")) {
                break;
            }
            
            try {
                String[] partes = entrada.split(",");
                if (partes.length == 2) {
                    double coeficiente = Double.parseDouble(partes[0]);
                    int exponente = Integer.parseInt(partes[1]);
                    polinomio.agregarTermino(coeficiente, exponente);
                } else {
                    System.out.println("Formato incorrecto. Use: coeficiente,exponente");
                }
            } catch (NumberFormatException e) {
                System.out.println("Error: coeficiente y exponente deben ser números válidos.");
            }
        }
        
        // Mostrar el polinomio ingresado
        System.out.println("\nPOLINOMIO INGRESADO:");
        polinomio.mostrarPolinomio();
        
        // Generar tabla de valores
        System.out.println("\nTABLA DE VALORES:");
        System.out.println("x\t|\tP(x)");
        System.out.println("-----------------------");
        
        for (double x = 0.0; x <= 5.0; x += 0.5) {
            double resultado = polinomio.evaluar(x);
            System.out.printf("%.1f\t|\t%.4f\n", x, resultado);
        }
        
        scanner.close();
    }
}

```
# Actividad 04: Polinomio con Lista Enlazada Circular 
```
package ejPracticos;

/**
 *
 * @author Diana Mabel Garcia Martinez
 *          1224100672
 *          diabegarciamtz@gmail.com    18/10/25
 */
// Clase genérica para polinomio con lista circular
public class PolinomioCircular<C extends Number, E extends Number> {
    private TerminoCircular<C, E> ultimo; // Referencia al último nodo
    
    public PolinomioCircular() {
        this.ultimo = null;
    }
    
    // Agregar término y mantener la circularidad
    public void agregarTermino(C coeficiente, E exponente) {
        TerminoCircular<C, E> nuevoTermino = new TerminoCircular<>(coeficiente, exponente);
        
        if (ultimo == null) {
            // Lista vacía - el nuevo nodo apunta a sí mismo
            nuevoTermino.siguiente = nuevoTermino;
            ultimo = nuevoTermino;
        } else {
            // Insertar después del último nodo
            nuevoTermino.siguiente = ultimo.siguiente;
            ultimo.siguiente = nuevoTermino;
            ultimo = nuevoTermino;
        }
    }
    
    // Recorrer la lista circular
    public void recorrerCircular() {
        if (ultimo == null) {
            System.out.println("La lista está vacía.");
            return;
        }
        
        System.out.print("Polinomio (recorrido circular): ");
        TerminoCircular<C, E> actual = ultimo.siguiente; // Comenzar desde el primero
        TerminoCircular<C, E> inicio = actual; // Guardar referencia al inicio
        
        boolean primerTermino = true;
        
        do {
            // Mostrar el término actual
            double coef = actual.coeficiente.doubleValue();
            int exp = actual.exponente.intValue();
            
            if (coef != 0) {
                if (!primerTermino && coef > 0) {
                    System.out.print(" + ");
                } else if (coef < 0) {
                    System.out.print(" - ");
                }
                
                double coefAbs = Math.abs(coef);
                
                if (exp == 0) {
                    System.out.print(coefAbs);
                } else if (exp == 1) {
                    System.out.print(coefAbs + "x");
                } else {
                    System.out.print(coefAbs + "x^" + exp);
                }
                
                primerTermino = false;
            }
            
            actual = actual.siguiente;
        } while (actual != inicio); // Parar cuando volvamos al inicio
        
        System.out.println();
    }
    
    // Evaluar el polinomio
    public double evaluar(double x) {
        if (ultimo == null) return 0.0;
        
        double resultado = 0.0;
        TerminoCircular<C, E> actual = ultimo.siguiente; // Comenzar desde el primero
        TerminoCircular<C, E> inicio = actual;
        
        do {
            double coef = actual.coeficiente.doubleValue();
            double exp = actual.exponente.doubleValue();
            resultado += coef * Math.pow(x, exp);
            actual = actual.siguiente;
        } while (actual != inicio);
        
        return resultado;
    }
}

```
```
package ejPracticos;

/**
 *
 * @author Diana Mabel Garcia Martinez
 *          1224100672
 *          diabegarciamtz@gmail.com    18/10/25
 */
import java.util.Scanner;

public class Actividad04 {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        PolinomioCircular<Double, Integer> polinomio = new PolinomioCircular<>();
        
        System.out.println("POLINOMIO CON LISTA CIRCULAR");
        System.out.println("Ingrese los términos en formato: coeficiente,exponente");
        System.out.println("Ejemplo: para 3x^4 - 4x^2 + 11 ingrese: 3,4 -4,2 11,0");
        System.out.println("Ingrese 'fin' para terminar");
        
        // Leer términos del polinomio
        while (true) {
            System.out.print("Ingrese término (coeficiente,exponente): ");
            String entrada = scanner.nextLine().trim();
            
            if (entrada.equalsIgnoreCase("fin")) {
                break;
            }
            
            try {
                String[] partes = entrada.split(",");
                if (partes.length == 2) {
                    double coeficiente = Double.parseDouble(partes[0]);
                    int exponente = Integer.parseInt(partes[1]);
                    polinomio.agregarTermino(coeficiente, exponente);
                } else {
                    System.out.println("Formato incorrecto. Use: coeficiente,exponente");
                }
            } catch (NumberFormatException e) {
                System.out.println("Error: coeficiente y exponente deben ser números válidos.");
            }
        }
        
        // Recorrer y mostrar el polinomio circular
        polinomio.recorrerCircular();
        
        // Evaluar para algunos valores
        System.out.println("\nEVALUACIÓN DEL POLINOMIO:");
        for (double x = 0.0; x <= 2.0; x += 0.5) {
            double resultado = polinomio.evaluar(x);
            System.out.printf("P(%.1f) = %.4f\n", x, resultado);
        }
        
        scanner.close();
    }
}

```
# Actividad 05: Lista Doblemente Enlazada de Caracteres
```
package ejPracticos;

/**
 *
 * @author Diana Mabel Garcia Martinez
 *          1224100672
 *          diabegarciamtz@gmail.com    18/10/25
 */
// Clase genérica para nodo doblemente enlazado
public class NodoDoble<T extends Comparable<T>> {
    public T dato;
    public NodoDoble<T> anterior;
    public NodoDoble<T> siguiente;
    
    public NodoDoble(T dato) {
        this.dato = dato;
        this.anterior = null;
        this.siguiente = null;
    }
}

```

```
package ejPracticos;

/**
 *
 * @author Diana Mabel Garcia Martinez
 *          1224100672
 *          diabegarciamtz@gmail.com    18/10/25
 */
// Clase genérica para lista doblemente enlazada
public class ListaDoble<T extends Comparable<T>> {
    private NodoDoble<T> cabeza;
    private NodoDoble<T> cola;
    
    public ListaDoble() {
        this.cabeza = null;
        this.cola = null;
    }
    
    // Insertar dato al final
    public void insertarAlFinal(T dato) {
        NodoDoble<T> nuevoNodo = new NodoDoble<>(dato);
        
        if (cabeza == null) {
            cabeza = nuevoNodo;
            cola = nuevoNodo;
        } else {
            cola.siguiente = nuevoNodo;
            nuevoNodo.anterior = cola;
            cola = nuevoNodo;
        }
    }
    
    // Mostrar lista desde el inicio
    public void mostrarDesdeInicio() {
        NodoDoble<T> actual = cabeza;
        System.out.print("Lista desde inicio: ");
        
        while (actual != null) {
            System.out.print(actual.dato + " ");
            actual = actual.siguiente;
        }
        System.out.println();
    }
    
    // Mostrar lista desde el final
    public void mostrarDesdeFinal() {
        NodoDoble<T> actual = cola;
        System.out.print("Lista desde final: ");
        
        while (actual != null) {
            System.out.print(actual.dato + " ");
            actual = actual.anterior;
        }
        System.out.println();
    }
    
    // Ordenamiento por inserción genérico
    public void ordenarPorInsercion() {
        if (cabeza == null || cabeza.siguiente == null) return;
        
        NodoDoble<T> ordenado = null; // Lista ordenada
        NodoDoble<T> actual = cabeza;
        
        while (actual != null) {
            NodoDoble<T> siguiente = actual.siguiente;
            
            // Insertar actual en la lista ordenada
            if (ordenado == null || ordenado.dato.compareTo(actual.dato) >= 0) {
                // Insertar al inicio
                actual.siguiente = ordenado;
                if (ordenado != null) ordenado.anterior = actual;
                actual.anterior = null;
                ordenado = actual;
            } else {
                // Buscar posición de inserción
                NodoDoble<T> temp = ordenado;
                while (temp.siguiente != null && temp.siguiente.dato.compareTo(actual.dato) < 0) {
                    temp = temp.siguiente;
                }
                
                // Insertar después de temp
                actual.siguiente = temp.siguiente;
                if (temp.siguiente != null) {
                    temp.siguiente.anterior = actual;
                }
                temp.siguiente = actual;
                actual.anterior = temp;
            }
            
            actual = siguiente;
        }
        
        // Actualizar cabeza y cola
        cabeza = ordenado;
        
        // Encontrar la nueva cola
        cola = cabeza;
        while (cola != null && cola.siguiente != null) {
            cola = cola.siguiente;
        }
    }
    
    // Método para obtener el tamaño de la lista
    public int obtenerTamano() {
        int contador = 0;
        NodoDoble<T> actual = cabeza;
        while (actual != null) {
            contador++;
            actual = actual.siguiente;
        }
        return contador;
    }
}

```

```
package ejPracticos;

/**
 *
 * @author Diana Mabel Garcia Martinez
 *          1224100672
 *          diabegarciamtz@gmail.com    18/10/25
 */
import java.util.Scanner;

public class Actividad05 {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        ListaDoble<Character> lista = new ListaDoble<>();
        
        System.out.print("Ingrese una cadena de texto: ");
        String cadena = scanner.nextLine();
        
        // Construir la lista con cada carácter
        for (char c : cadena.toCharArray()) {
            if (c != ' ') { // Ignorar espacios
                lista.insertarAlFinal(c);
            }
        }
        
        // Mostrar lista original
        System.out.println("\nLISTA ORIGINAL:");
        lista.mostrarDesdeInicio();
        lista.mostrarDesdeFinal();
        
        // Ordenar la lista
        System.out.println("\nORDENANDO LA LISTA...");
        lista.ordenarPorInsercion();
        
        // Mostrar lista ordenada
        System.out.println("\nLISTA ORDENADA ALFABÉTICAMENTE:");
        lista.mostrarDesdeInicio();
        
        scanner.close();
    }
}

```
