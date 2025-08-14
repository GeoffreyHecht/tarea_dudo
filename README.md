# Kata TDD: Simulador del Juego Dudo Chileno

## Contexto
El Dudo es un juego tradicional chileno que se juega con dados en "cachos". Su tarea es implementar un simulador que maneje la lógica central del juego usando TDD. Como parece haber tantas variantes de reglas como jugadores, vamos a usar las reglas de la siguiente página: https://www.donpichuncho.cl/aprende-a-jugar-dudo-en-cacho

## Objetivos
- Aplicar TDD con Python3 y pytest y pytest-mock
- Usar mocking cuando sea apropiado
- Diseñar clases con responsabilidades claras
- Manejar lógica de juego compleja paso a paso

## Requerimientos Funcionales

### Sistema de Dados y Pintas
Implementa una clase `Dado` que:
- Genere valores del 1 al 6
- Use las denominaciones tradicionales:
  - 1: "As", 2: "Tonto", 3: "Tren", 4: "Cuadra", 5: "Quina", 6: "Sexto"

Implementa una clase `Cacho` que:
- Contenga 5 dados
- Permita "agitar" (regenerar valores)
- Oculte/muestre los dados según el estado del juego

### Validador de Apuestas
Implementa una clase `ValidadorApuesta` que verifique:
- Si una nueva apuesta es válida (mayor cantidad o pinta superior)
- Las reglas especiales de los Ases:
  - Al cambiar A ases: dividir cantidad actual por 2 (par: +1, impar: redondear arriba)
  - Al cambiar DE ases: multiplicar por 2 y sumar 1
- Que no se pueda partir con Ases (excepto con un dado)

### Contador de Pintas
Implementa una clase `ContadorPintas` que:
- Cuente apariciones de una pinta específica en todos los dados
- Trate los Ases como comodines (suman a cualquier pinta apostada)
- Maneje el caso especial cuando los Ases NO son comodines (ronda de un dado)

### Árbitro del Juego
Implementa una clase `ArbitroRonda` que:
- Determine el resultado cuando un jugador "duda"
- Maneje la lógica de "calzar" (debe ser exacto)
- Decida quién pierde/gana un dado
- Valide las condiciones para "calzar" (mitad de dados en juego O jugador con un dado)

### Gestor de Partida
Implementa una clase `GestorPartida` que:
- Administre múltiples jugadores y sus dados
- Determine quién inicia cada ronda
- Maneje el flujo de turnos
- Detecte cuándo alguien queda con un dado (para activar reglas especiales)

## Aspectos Técnicos

### Ejecución de los tests
Para ejecutar los tests, después de instalar las dependencias con pip o equivalente, pueden usar:
```
pytest
o
python3 -m pytest
```

Para saber el detalle de la cobertura de los tests, pueden ejecutar:
```
pytest --cov=src --cov-report=term-missing
o
python3 -m pytest --cov=src --cov-report=term-missing
```


### Mocking Requerido
- **Generador de números aleatorios**: Para hacer pruebas deterministas
- **Aislamiento de las pruebas**: Mocks para aislar las diferentes partes de la lógica de negocio (excluyendo las clases orientadas a orquestaciones que pueden no hacer mocking)

### Estructura Sugerida
Pueden adaptar la organización y añadir archivos, pero deben mantener un código organizado.
```
src/
├── juego/
│   ├── dado.py
│   ├── cacho.py
│   ├── validador_apuesta.py
│   ├── contador_pintas.py
│   ├── arbitro_ronda.py
│   └── gestor_partida.py
├── servicios/
│   └── generador_aleatorio.py
tests/
├── test_dado.py
├── test_cacho.py
├── test_validador_apuesta.py
├── test_contador_pintas.py
├── test_arbitro_ronda.py
└── test_gestor_partida.py
```

## Metodología TDD - Commits Obligatorios

**IMPORTANTE**: Para evaluar que siguieron TDD correctamente, deben hacer commits siguiendo el ciclo Rojo-Verde-Refactor:

### Patrón de Commits Requerido
Para cada funcionalidad, deben hacer **exactamente 3 commits** en este orden:

1. **🔴 ROJO**: `git commit -m "RED: test para [funcionalidad] - falla como esperado"`
   - Solo el test, sin implementación
   - El test debe fallar por la razón correcta
   - Ejecutar `pytest` debe mostrar el fallo

2. **🟢 VERDE**: `git commit -m "GREEN: implementación mínima para [funcionalidad]"`
   - Código mínimo para hacer pasar el test
   - Ejecutar `pytest` debe mostrar todos los tests pasando
   - No importa si el código es "feo" en esta etapa

3. **🔵 REFACTOR**: `git commit -m "REFACTOR: mejora código de [funcionalidad]"`
   - Mejorar la implementación sin cambiar funcionalidad
   - Todos los tests siguen pasando
   - Solo si hay algo que refactorizar (sino omitir este commit)

### Ejemplo de Secuencia de Commits
```
 RED: test para generar valor aleatorio en dado - falla como esperado
 GREEN: implementación mínima para generar valor aleatorio en dado  
 REFACTOR: mejora método de generación con dependency injection
 RED: test para denominar pinta del dado - falla como esperado
 GREEN: implementación mínima para denominar pinta del dado
 ...
```

## Entregables
1. Código fuente con cobertura de pruebas > 90%
2. Todas las pruebas deben pasar
3. Implementación que siga principios SOLID
4. Historial de commits en el formato descrito
5. README con instrucciones de ejecución
