El desafío consiste en seleccionar un conjunto mínimo de estaciones de radio que aseguren que todos los estados de un país puedan escuchar música de Beyoncé. Dado un conjunto de estaciones y los estados que cada una cubre, debemos encontrar el subconjunto mínimo de estaciones que cubra todos los estados.  
estaciones = {  
    'Estacion 1': {'Estado 1', 'Estado 2', 'Estado 3'},  
    'Estacion 2': {'Estado 3', 'Estado 4', 'Estado 5'},  
    'Estacion 3': {'Estado 1', 'Estado 6', 'Estado 7'},  
    'Estacion 4': {'Estado 2', 'Estado 6', 'Estado 8'},  
    'Estacion 5': {'Estado 3', 'Estado 8', 'Estado 9'},  
    'Estacion 6': {'Estado 4', 'Estado 10', 'Estado 11'},  
    'Estacion 7': {'Estado 5', 'Estado 11', 'Estado 12'},  
    'Estacion 8': {'Estado 6', 'Estado 13', 'Estado 14'},  
    'Estacion 9': {'Estado 7', 'Estado 14', 'Estado 15'},  
    'Estacion 10': {'Estado 8', 'Estado 16', 'Estado 17'},  
    'Estacion 11': {'Estado 9', 'Estado 17', 'Estado 18'},  
    'Estacion 12': {'Estado 10', 'Estado 18', 'Estado 19'},  
    'Estacion 13': {'Estado 11', 'Estado 19', 'Estado 20'}  
}


estados = {'Estado 1', 'Estado 2', 'Estado 3', 'Estado 4', 'Estado 5', 'Estado 6', 'Estado 7', 'Estado 8',   
           'Estado 9', 'Estado 10', 'Estado 11', 'Estado 12', 'Estado 13', 'Estado 14', 'Estado 15', 'Estado 16',  
           'Estado 17', 'Estado 18', 'Estado 19', 'Estado 20'}  
Podemos usar dos enfoques:

    Mínimo Local: Selección de estaciones de manera iterativa, eligiendo la estación que cubra más estados no cubiertos en cada paso.
    
    def minimo_local(estaciones, estados):
    estados_cubiertos = set()
    estaciones_seleccionadas = []
    while estados_cubiertos != estados:
        mejor_estacion = None
        mejor_cobertura = set()
        for estacion, estados_radio in estaciones.items():
            cobertura = estados_radio - estados_cubiertos
            if len(cobertura) > len(mejor_cobertura):
                mejor_estacion = estacion
                mejor_cobertura = cobertura
        if mejor_estacion:
            estaciones_seleccionadas.append(mejor_estacion)
            estados_cubiertos.update(mejor_cobertura)
            del estaciones[mejor_estacion]
    return estaciones_seleccionadas

estaciones_seleccionadas_local = minimo_local(estaciones.copy(), estados)
print("Estaciones seleccionadas (mínimo local):", estaciones_seleccionadas_local)
    
    Mínimo Global: Búsqueda de la combinación óptima mediante fuerza bruta.

    def minimo_global(estaciones, estados):
    estaciones_combinadas = list(estaciones.items())
    mejor_comb = None
    mejor_num = len(estaciones) + 1
    for r in range(1, len(estaciones_combinadas) + 1):
        for combinacion in itertools.combinations(estaciones_combinadas, r):
            estados_cubiertos = set()
            for estacion, estados_radio in combinacion:
                estados_cubiertos.update(estados_radio)
            if estados_cubiertos == estados and len(combinacion) < mejor_num:
                mejor_comb = combinacion
                mejor_num = len(combinacion)
    return [estacion for estacion, _ in mejor_comb]

estaciones_seleccionadas_global = minimo_global(estaciones, estados)
print("Estaciones seleccionadas (mínimo global):", estaciones_seleccionadas_global)

Búsqueda Local: Más rápida pero puede no ser óptima.

Búsqueda Global: Garantiza una solución óptima, pero es más costosa en términos de tiempo y recursos.

La elección entre estos métodos depende de la necesidad de encontrar una solución rápida versus la necesidad de una solución óptima.
