# Régimen General CNV — Estados Contables y Moneda Extranjera

Dataset extraído de la **Comisión Nacional de Valores (CNV)** de Argentina con información contable y de moneda extranjera de todas las empresas registradas bajo el Régimen General, tanto en oferta pública como suspendidas preventivamente.

## Origen de los datos

Los datos fueron obtenidos mediante web scraping del sitio oficial de la CNV. Incluyen estados contables (balances) y posiciones en moneda extranjera reportadas por cada empresa.

**Fecha de extracción:** _02-19-2026_

## Estructura del repositorio

```
regimen_general/
├── all_balances.csv                          # Consolidado de todos los balances
├── En Oferta Publica/
│   ├── [Nombre Social]/
│   │   ├── balance_[CUIT]_[fecha].csv        # Balance desglosado por rubro
│   │   ├── balance_[CUIT]_[fecha].json       # Metadata del balance
│   │   └── moneda_extranjera_[CUIT]_[fecha].csv  # Posición en moneda extranjera
│   └── ...
└── Suspendida Preventivamente/
    ├── [Nombre Social]/
    │   ├── balance_[CUIT]_[fecha].csv
    │   ├── balance_[CUIT]_[fecha].json
    │   └── moneda_extranjera_[CUIT]_[fecha].csv
    └── ...
```

## Descripción de archivos

### `all_balances.csv`

Archivo consolidado con todos los balances en un único CSV.

| Campo | Descripción |
|-------|-------------|
| `Fecha` | Fecha del balance |
| `Cuit` | CUIT de la empresa |
| `Nombre Social` | Razón social |
| `Nro` | Número de rubro contable |
| `Rubro` | Descripción del rubro |
| `Monto` | Monto en la moneda indicada en el JSON correspondiente |

### `balance_[CUIT]_[fecha].csv`

Balance individual de cada empresa, desglosado por rubro.

| Campo | Descripción |
|-------|-------------|
| `Nro` | Número de rubro contable |
| `Rubro` | Descripción del rubro |
| `Monto` | Monto reportado |

### `balance_[CUIT]_[fecha].json`

Metadata asociada al balance.

```json
{
  "PeriodoBalance": "1",
  "FechaCierre": "2019-12-31T03:00:00.000Z",
  "FechaDesde": null,
  "FechaHasta": null,
  "Detalle": null,
  "TipoBalance": "Individual",
  "Moneda": "Peso (Argentina)",
  "UnidadMedida": "$",
  "NormasContablesAplicadas": "NIIF",
  "Link": "https://aif2.cnv.gov.ar/presentations/publicview/..."
}
```

| Campo | Descripción |
|-------|-------------|
| `PeriodoBalance` | Número de período del balance |
| `FechaCierre` | Fecha de cierre del ejercicio |
| `TipoBalance` | Individual o Consolidado |
| `Moneda` | Moneda de presentación (ej: Peso Argentina) |
| `UnidadMedida` | Símbolo de la unidad (ej: $) |
| `NormasContablesAplicadas` | Marco contable utilizado (ej: NIIF) |
| `Link` | Enlace a la presentación original en la CNV |

### `moneda_extranjera_[CUIT]_[fecha].csv`

Posición en moneda extranjera en formato CSV, con columnas de activo y pasivo por tipo.

```csv
Moneda,ActivoInversiones,ActivoExistencias,PasivoDeudasComerciales,PasivoOtrasDeudas,ActivoCreditosComerciales,PasivoPrestamosBancarios,TieneActivosMonedaExtranjera
Dólar (E.U.A.),0,0,0,0,0,0,1
```

| Campo | Descripción |
|-------|-------------|
| `Moneda` | Tipo de moneda extranjera (ej: Dólar E.U.A., Euro) |
| `ActivoInversiones` | Inversiones en moneda extranjera |
| `ActivoExistencias` | Existencias/inventarios en moneda extranjera |
| `ActivoCreditosComerciales` | Créditos comerciales en moneda extranjera |
| `PasivoDeudasComerciales` | Deudas comerciales en moneda extranjera |
| `PasivoOtrasDeudas` | Otras deudas en moneda extranjera |
| `PasivoPrestamosBancarios` | Préstamos bancarios en moneda extranjera |
| `TieneActivosMonedaExtranjera` | Flag (1/0) indicando si posee activos en esa moneda |

## Categorías

| Categoría | Descripción |
|-----------|-------------|
| **En Oferta Pública** | Empresas con autorización vigente para oferta pública de valores |
| **Suspendida Preventivamente** | Empresas cuya autorización de oferta pública fue suspendida de forma preventiva |

## Uso

El dataset permite, entre otras cosas:

- Analizar la composición de activos y pasivos de empresas bajo régimen general de la CNV.
- Comparar posiciones en moneda extranjera entre empresas.
- Identificar patrones contables por sector o categoría regulatoria.
- Construir dashboards de monitoreo financiero del mercado de capitales argentino.

## Disclaimer

Este dataset se publica con fines informativos y de investigación. Los datos provienen de fuentes públicas de la CNV. No constituye asesoramiento financiero ni recomendación de inversión. La información puede contener errores derivados del proceso de extracción. Ante cualquier duda, consultar la [fuente oficial](https://www.cnv.gob.ar/).
