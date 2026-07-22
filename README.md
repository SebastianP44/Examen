## Ordenamiento

Bubble 

La idea: Compara elementos adyacentes de dos en dos y los intercambia si están en el orden incorrecto. En cada pasada completa, el elemento más grande "flota" hasta el final.

### Codigo:

public static void bubbleSort(int[] arr) {
    int n = arr.length;
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - 1 - i; j++) {
            if (arr[j] > arr[j + 1]) {
                // Intercambio (swap)
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
}

Selection

### Codigo:

La idea: Busca el elemento mínimo en la parte no ordenada del arreglo y lo intercambia con la primera posición disponible. Va construyendo la lista ordenada de izquierda a derecha.

public static void selectionSort(int[] arr) {
    int n = arr.length;
    for (int i = 0; i < n - 1; i++) {
        int indiceMinimo = i;
        
        // Busca el menor elemento en el resto del arreglo
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[indiceMinimo]) {
                indiceMinimo = j;
            }
        }
        
        // Hace un solo intercambio por cada pasada
        int temp = arr[indiceMinimo];
        arr[indiceMinimo] = arr[i];
        arr[i] = temp;
    }
}

Insercion

La idea: Similar a como ordenas un mazo de cartas en la mano. Toma un elemento de la parte no ordenada y lo inserta en la posición correcta dentro de la sublista que ya está ordenada a la izquierda.

### Codigo:

public static void insertionSort(int[] arr) {
    int n = arr.length;
    for (int i = 1; i < n; i++) {
        int clave = arr[i];
        int j = i - 1;

        // Desplaza los elementos mayores a la clave una posición a la derecha
        while (j >= 0 && arr[j] > clave) {
            arr[j + 1] = arr[j];
            j--;
        }
        // Coloca la clave en su lugar correcto
        arr[j + 1] = clave;
    }
}


## Busquedas

Lineal

¿Qué hace? Recorre el arreglo elemento por elemento desde el inicio hasta encontrar el valor o llegar al final.Cuándo usarla: En arreglos desordenados o pequeños.Complejidad: $O(n)$

### Codigo: 

public static int busquedaLineal(int[] arreglo, int objetivo) {
    for (int i = 0; i < arreglo.length; i++) {
        if (arreglo[i] == objetivo) {
            return i; // Retorna la posición (índice) donde se encontró
        }
    }
    return -1; // Retorna -1 si el elemento no existe
}

Binaria

¿Qué hace? Aplica la estrategia de "divide y vencerás". Mira el elemento del centro y descarta la mitad del arreglo en cada paso según si el valor buscado es mayor o menor.Cuándo usarla: El arreglo DEBE estar ordenado.Complejidad: $O(log n)$

codigo: 

public static int busquedaBinaria(int[] arreglo, int objetivo) {
    int inicio = 0;
    int fin = arreglo.length - 1;

    while (inicio <= fin) {
        int medio = inicio + (fin - inicio) / 2;

        if (arreglo[medio] == objetivo) {
            return medio; // Elemento encontrado
        }

        if (arreglo[medio] < objetivo) {
            inicio = medio + 1; // Buscar en la mitad derecha
        } else {
            fin = medio - 1;    // Buscar en la mitad izquierda
        }
    }
    return -1; // No encontrado
}

Tabla hash:

¿Qué hace? Utiliza una función matemática (función hash) para transformar la clave en una dirección de memoria directa, obteniendo el dato de forma instantánea sin recorrer ni comparar arreglos.Cuándo usarla: Búsquedas ultra rápidas por clave/identificador (HashMap o HashSet).Complejidad: $O(1)$ promedio

codigo:

import java.util.HashMap;

public static String busquedaHash(HashMap<Integer, String> tablaHash, int clave) {
    // Acceso directo instantáneo usando la clave
    if (tablaHash.containsKey(clave)) {
        return tablaHash.get(clave);
    }
    return null; // Clave no encontrada
}

## controladores set y map 

package controllers;

import java.util.LinkedHashSet;
import java.util.List;
import java.util.Map;
import java.util.Set;
import java.util.TreeMap;
import java.util.TreeSet;

import models.LabSupply;

public class InventoryController {

  public Set<LabSupply> filterAndSortSuplies(List<LabSupply> supplies, int minimunStock) {
    Set<LabSupply> orden = new TreeSet<>(
        (t1, t2) -> {
          int stock = Integer.compare(t1.getStock(), t2.getStock());
          if (stock != 0) {
            return stock;
          }

          int code = t2.getCode().compareToIgnoreCase(t1.getCode());
          if (code == 0) {
            return 0;
          }

          int manager = t2.getManager().compareToIgnoreCase(t1.getManager());
          if (manager != 0) {
            return manager;

          }
          return code;

        });
    for (LabSupply sup : supplies) {
      if (sup.getStock() >= minimunStock) {
        orden.add(sup);
      }

    }
    return orden;
  }

  public Map<String, Set<String>> groupCodesByStock(List<LabSupply> supplies) {
    Map<String, Set<String>> agruparCodigos = new TreeMap<>();
    agruparCodigos.put("High", new LinkedHashSet<>());
    agruparCodigos.put("Medium", new LinkedHashSet<>());
    agruparCodigos.put("Low", new LinkedHashSet<>());

    for (LabSupply sup : supplies) {
      String stock = sup.getCode().substring(3);
      if (sup.getStock() >= 80) {
        agruparCodigos.get("High").add(stock);

      } else if (sup.getStock() <= 45 && sup.getStock() >= 20) {
        agruparCodigos.get("Medium").add(stock);
      } else if (sup.getStock() == 8) {
        agruparCodigos.get("Low").add(stock);
      }

    }
    return agruparCodigos;

  }

}

### models 

package models;

public class LabSupply {

  private String code;
  private String manager;
  private int stock;

  public LabSupply() {
  }

  public LabSupply(String code, String manager, int stock) {
    this.code = code;
    this.manager = manager;
    this.stock = stock;
  }

  public String getCode() {
    return code;
  }

  public void setCode(String code) {
    this.code = code;
  }

  public String getManager() {
    return manager;
  }

  public void setManager(String manager) {
    this.manager = manager;
  }

  public int getStock() {
    return stock;
  }

  public void setStock(int stock) {
    this.stock = stock;
  }
}
## App main 

codigo: 

import java.util.ArrayList;
import java.util.List;
import java.util.Map;
import java.util.Set;

import controllers.InventoryController;
import models.LabSupply;

public class App {
    public static void main(String[] args) throws Exception {

        List<LabSupply> supplies = new ArrayList<>();
        supplies.add(new LabSupply("IN-205-104", "Ana Torres", 80));
        supplies.add(new LabSupply("IN-201-101", "Luis Mora", 15));
        supplies.add(new LabSupply("IN-208-108", "Carlos Vega", 45));
        supplies.add(new LabSupply("in-205-104", "Ana Torres", 80));
        supplies.add(new LabSupply("IN-203-103", "Luis Andrade", 30));
        supplies.add(new LabSupply("IN-207-107", "Mateo Rojas", 8));
        supplies.add(new LabSupply("IN-202-102", "Sofia Cordero", 55));
        supplies.add(new LabSupply("IN-206-106", "Carlos Mendez", 90));
        supplies.add(new LabSupply("IN-204-105", "Ana Molina", 20));

        System.out.println("\nMetodo A");
        InventoryController controlador = new InventoryController();
        Set<LabSupply> orden = controlador.filterAndSortSuplies(supplies, 55);
        for (LabSupply sup : orden) {
            System.out.println("- " + sup.getCode() + "(" + sup.getManager() + ") ");

        }
        System.out.println("\nMetodo b ");
        Map<String, Set<String>> agrupadoPorCodigo = controlador.groupCodesByStock(supplies);
        for (Map.Entry<String, Set<String>> entry : agrupadoPorCodigo.entrySet()) {
            System.out.println("-" + entry.getKey() + entry.getValue());

        }

    }
}


### HashSet: 
(La opción por defecto)Es la implementación más rápida. Guarda los elementos usando una función Hash, por lo que no garantiza ningún orden. El orden puede incluso cambiar con el tiempo cuando el conjunto se reestructura.Ventaja: Operaciones de inserción, eliminación y búsqueda en tiempo constante $O(1)$.Desventaja: Pierdes el control de cómo se listan los elementos.

### LinkedHashSet:
  (Conservador del orden)Hereda de HashSet, pero internamente mantiene una lista doblemente enlazada que conecta todos sus elementos.Ventaja: Mantiene la velocidad $O(1)$ de HashSet, pero cuando recorres el conjunto, los elementos salen en el mismo orden exacto en que los agregaste.Desventaja: Consume un poco más de memoria para mantener los punteros de la lista.

### TreeSet:
 (El conjunto ordenado)Internamente utiliza un Árbol Rojo-Negro (TreeMap). Cada vez que agregas un elemento, lo ubica en la posición correcta según su orden natural (numérico, alfabético) o mediante un Comparator personalizado.Ventaja: El conjunto siempre está ordenado. Permite búsquedas por rango (como "elementos menores a $X$").Desventaja: Es más lento que los otros dos ($O(log n)$ por inserción/búsqueda). No permite agregar null porque necesita comparar los elementos entre sí.

#   E x a m e n  
 