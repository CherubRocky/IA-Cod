función A_Estrella(grafo, inicio, objetivo, heuristica):
    // La frontera es una Cola de Prioridad Mínima que ordena por f_score
    frontera = nueva ColaDePrioridadMinima()
    frontera.insertar(inicio, heuristica(inicio, objetivo))
    
    // Diccionario para reconstruir el camino final
    vino_de = nuevo Mapa()
    
    // Mapa de costos reales (g_score): cuánto cuesta llegar desde el inicio
    costo_g = nuevo Mapa()
    costo_g[inicio] = 0
    
    mientras frontera no esté vacía:
        // Extraemos el nodo con el f_score más bajo
        actual = frontera.extraer_minimo() 
        
        // Si llegamos a la meta, reconstruimos y devolvemos la ruta
        si actual == objetivo:
            retornar reconstruir_camino(vino_de, actual)
            
        // Exploramos los vecinos del nodo actual
        para cada (vecino, peso_arista) en grafo.obtener_vecinos(actual):
            // Calculamos el costo real para llegar a este vecino pasando por 'actual'
            nuevo_costo_g = costo_g[actual] + peso_arista
            
            // Si este camino es mejor que el anterior (o si el vecino es nuevo)
            si vecino no existe en costo_g o nuevo_costo_g < costo_g[vecino]:
                // Actualizamos los datos del vecino
                costo_g[vecino] = nuevo_costo_g
                f_score = nuevo_costo_g + heuristica(vecino, objetivo)
                
                // Lo añadimos o actualizamos en la cola de prioridad
                frontera.insertar(vecino, f_score)
                
                // Guardamos de dónde venimos para poder trazar la ruta de regreso
                vino_de[vecino] = actual
                
    // Si la cola se vacía sin llegar al objetivo, no hay camino posible
    retornar Nulo

// --- Función auxiliar para trazar la ruta de regreso ---
función reconstruir_camino(vino_de, actual):
    camino = [actual]
    mientras actual exista como clave en vino_de:
        actual = vino_de[actual]
        camino.agregar_al_inicio(actual) // Para que quede ordenado de inicio a fin
    retornar camino