# Plan de implementación

Documento de infraestructura, módulos y estado del proyecto. Sirve como guía para
completar el repositorio y como punto de partida para el siguiente grupo.

## 1. Restricciones y decisiones

| Tema | Decisión |
|---|---|
| Lenguaje | Java 17+ (probado con Java 21). |
| Frameworks de aplicación | Ninguno. Solo el JDK. |
| Build | Scripts `bash` + `javac` (sin Maven ni Gradle). |
| Pruebas | Runner propio (cero dependencias). |
| Backend web | `com.sun.net.httpserver.HttpServer` del JDK. |
| Serialización | `Json` escrito a mano (sin librerías). |
| Frontend | HTML/CSS/JS estático servido por el backend. |
| Decoradores | **Implementados por el equipo**, jamás de framework. |
| Idioma | Código e identificadores en inglés; interfaz en español. |

## 2. Infraestructura del repositorio

```
decoratorrrr/
├── README.md
├── docs/
│   ├── CASE_STUDY.md
│   └── IMPLEMENTATION_PLAN.md
├── scripts/            # build.sh, run-demo.sh, run-server.sh, test.sh
├── src/coffeeshop/     # código de producción
├── test/coffeeshop/    # pruebas
└── frontend/           # index.html, styles.css, app.js, assets/
```

Convenciones:

- Paquete raíz `coffeeshop`, subpaquetes `domain`, `application`, `infra`.
- Un tipo público por archivo.
- Autoría de commits con correos `noreply` de GitHub, sin `Co-authored-by`.
- Flujo git: una rama por integrante (`jeni`, `drako`, `miguel`) integrada a `main`
  mediante *pull requests* aprobados sin comentarios.

## 3. Módulos

### M0 — Infraestructura y documentación
`.gitignore`, estructura de carpetas, scripts de build/ejecución/test, `README.md`,
`CASE_STUDY.md` y este plan.

**Estado: implementado (base).** El contenido de estado se actualiza al final de cada módulo.

### M1 — Dominio: Component y bebidas
- `Beverage` (interface): `getDescription()`, `getCost()`, `getIngredients()`.
- `BaseBeverage` (abstract): estado `description` y `cost`.
- `Espresso`, `Americano`, `Latte`, `Tea`.

### M2 — Decoradores
- `BeverageDecorator` (abstract): guarda un `Beverage` y delega.
- Decoradores concretos del enunciado: `ExtraShotDecorator`, `MilkDecorator`,
  `CaramelDecorator`, `VanillaDecorator`, `WhippedCreamDecorator`, `SizeDecorator`.
- Decoradores propios que refuerzan el patrón: `HoneyDecorator`, `OatMilkDecorator`,
  `DecafDecorator`, `IcedDecorator`, `HappyHourDecorator` (costo negativo).

### M3 — Capa de aplicación
- Enums `DrinkType`, `Size`, `ExtraType`.
- Modelos `OrderRequest`, `OrderResult`, `ReceiptLine`, `CatalogItem`.
- `BeverageCatalog`: fábrica de bebidas y decoradores.
- `OrderService`: construye la cadena decorada y arma el resultado.

### M4 — Demo y pruebas
- `DemoMain`: pedidos de ejemplo por consola.
- `coffeeshop.tests.TestRunner`: micro-framework de aserciones y suite de pruebas
  (costo recursivo, descripción, combinaciones, decorador con costo negativo).

### M5 — Backend HTTP
- `CoffeeShopServer` + handlers: `GET /api/catalog`, `POST /api/order`,
  `GET /api/orders` (historial), estáticos del `frontend/`.
- `Json`: serializador/parser mínimo.

### M6 — Frontend gráfico
- `index.html`, `styles.css`, `app.js`, `assets/cup.svg`.
- Taza SVG reactiva (color por bebida, escala por tamaño, capas por decorador).
- Visualizador de la cadena decoradora anidada.
- Recibo con desglose por capa y total en vivo.
- Tema visual "cafetería" (crema, espresso, caramelo, ámbar), interfaz en español.

## 4. Roadmap para el siguiente grupo (pendientes)

| # | Pendiente | Notas |
|---|---|---|
| M7 | **Historial de pedidos y persistencia** | El historial ya viaja en memoria (`/api/orders`); falta persistirlo a archivo o base de datos, con paginación y totales. |
| M8 | **Extras de interfaz** | Conmutador ES/EN, tema claro/oscuro, exportar/imprimir el recibo, accesibilidad avanzada. |
| M9 | **Decoradores adicionales** | P. ej. `LoyaltyDecorator` (descuento por cliente frecuente), `SeasonalFlavorDecorator`, combos. |
| M10 | **Constructor de decoradores en la UI** | Permitir al usuario combinar y reordenar capas y ver el efecto en la cadena. |

## 5. Cómo compilar y verificar

```bash
bash scripts/build.sh
bash scripts/test.sh
bash scripts/run-demo.sh
bash scripts/run-server.sh
```
