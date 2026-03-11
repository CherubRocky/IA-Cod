func AEstrella(grafo, inicio, objetivo, heuristica):
    frontera = nueva ColaDePrioridadMinima()
    frontera.insertar(inicio, heuristica(inicio, objetivo))
    
    vino_de = nuevo diccionario()
    
    // Mapa de costos reales (g_score): cuánto cuesta llegar desde el inicio
    costo_g = nuevo Mapa()
    costo_g[inicio] = 0
    
    mientras frontera no esté vacía:
        // Extraemos el nodo con el f_score más bajo
        actual = frontera.extraer_minimo() 
        
        // Si llegamos a la meta, reconstruimos y devolvemos la ruta
        si actual == objetivo:
            retornar reconstruir_camino(vino_de, actual)
            
        para cada (vecino, peso_arista) en grafo.obtener_vecinos(actual):
            nuevo_costo_g = costo_g[actual] + peso_arista
            
            si vecino no existe en costo_g o nuevo_costo_g < costo_g[vecino]:
                costo_g[vecino] = nuevo_costo_g
                f_score = nuevo_costo_g + heuristica(vecino, objetivo)
                
                frontera.insertar(vecino, f_score)
                
                vino_de[vecino] = actual
                
    retornar Nulo

función reconstruir_camino(vino_de, actual):
    camino = [actual]
    mientras actual exista como clave en vino_de:
        actual = vino_de[actual]
        camino.agregar_al_inicio(actual) // Para que quede ordenado de inicio a fin
    retornar camino