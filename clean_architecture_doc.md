# Arquitectura Limpia - Frontend Angular

## 📋 Tabla de Contenidos

1. [Introducción](#introducción)
2. [Estructura de Carpetas](#estructura-de-carpetas)
3. [Capas de la Arquitectura](#capas-de-la-arquitectura)
4. [Implementación Paso a Paso](#implementación-paso-a-paso)
5. [API de Pruebas con json-server](#api-de-pruebas-con-json-server)
6. [Ejemplo Completo: Detalle de Producto](#ejemplo-completo-detalle-de-producto)
7. [Buenas Prácticas](#buenas-prácticas)

---

## Introducción

Este proyecto implementa **Arquitectura Limpia** (Clean Architecture) en el frontend Angular, separando responsabilidades en tres capas principales:

- **Domain**: Modelos y entidades del negocio
- **Infrastructure**: Implementación de repositorios, APIs y mappers
- **Application**: Servicios orquestadores que coordinan la lógica de negocio

### Beneficios

✅ **Separación de responsabilidades**: Cada capa tiene un propósito claro  
✅ **Testeable**: Fácil de escribir tests unitarios  
✅ **Mantenible**: Cambios en una capa no afectan a las demás  
✅ **Escalable**: Fácil agregar nuevas features siguiendo el patrón  

---

## Estructura de Carpetas

```
frontend/stockmaster-client/src/app/
├── core/
│   ├── domain/
│   │   └── models/
│   │       └── product.model.ts          # Entidades del negocio
│   ├── infrastructure/
│   │   ├── http/
│   │   │   └── api.service.ts            # Cliente HTTP base
│   │   ├── repositories/
│   │   │   └── products.repository.ts    # Implementación repositorio
│   │   └── mappers/
│   │       └── product.mapper.ts         # Conversión DTO ↔ Domain
│   └── application/
│       └── products/
│           └── product-details.application-service.ts  # Orquestador
└── features/
    └── user/
        └── product-detail/
            ├── product-detail.component.ts
            ├── product-detail.component.html
            └── product-detail.component.css
```

---

## Capas de la Arquitectura

### 1️⃣ Domain (Núcleo del Negocio)

**Propósito**: Define las entidades y reglas de negocio.

**Características**:
- No depende de ninguna otra capa
- Solo TypeScript puro
- Define interfaces y tipos

**Ejemplo**:
```typescript
// product.model.ts
export interface Product {
  id: string;
  slug: string;
  name: string;
  brand: string;
  prices: PriceTier[];
  // ... más propiedades
}
```

### 2️⃣ Infrastructure (Implementación Técnica)

**Propósito**: Implementa la comunicación con APIs externas, bases de datos, etc.

**Características**:
- Implementa repositorios
- Maneja DTOs (Data Transfer Objects)
- Convierte datos externos a entidades del dominio

**Componentes**:
- **Repository**: Accede a datos externos
- **Mapper**: Convierte DTO → Domain Model
- **HTTP Service**: Cliente HTTP configurado

**Ejemplo**:
```typescript
// products.repository.ts
@Injectable({ providedIn: 'root' })
export class ProductsRepository {
  getBySlug(slug: string): Observable<Product | null> {
    return this.api.get<ProductDto[]>('products', { slug }).pipe(
      map(list => list[0] ? mapProductDtoToDomain(list[0]) : null)
    );
  }
}
```

### 3️⃣ Application (Orquestación)

**Propósito**: Coordina la lógica de negocio entre UI e Infrastructure.

**Características**:
- Usa repositorios
- Maneja estado (Signals)
- Expone métodos para la UI

**Ejemplo**:
```typescript
// product-details.application-service.ts
@Injectable({ providedIn: 'root' })
export class ProductDetailsApplicationService {
  private repo = inject(ProductsRepository);
  
  readonly product = computed(() => this.productSignal());
  readonly isLoading = computed(() => this.loadingSignal());
  
  setSlug(slug: string) {
    this.repo.getBySlug(slug).subscribe(/* ... */);
  }
}
```

### 4️⃣ Features (UI/Presentación)

**Propósito**: Componentes de Angular que muestran datos al usuario.

**Características**:
- Consume Application Services
- Solo se preocupa de la presentación
- No tiene lógica de negocio compleja

---

## Implementación Paso a Paso

### Paso 1: Crear el Modelo de Dominio

```typescript
// src/app/core/domain/models/product.model.ts

export type priceLabel = 'unit' | 'wholesale' | 'box';

export interface PriceTier {
  label: priceLabel;
  price: number;
  minQuantity?: number;
}

// Interfaz simplificada para cards/listados
export interface ProductCard {
  id: string;
  sku: string;
  name: string;
  brand?: string;
  prices: PriceTier[];
  images?: string[];
}

// Interfaz completa para detalles
export interface Product extends ProductCard {
  slug: string;
  categoryId: string;
  brand: string;
  unitPerBox: number;
  images: string[];
  isActive: boolean;
  createdAt: number;
  updatedAt: number;
  stockUnits: number;
  stockBoxes: number;
}
```

### Paso 2: Crear el Mapper

```typescript
// src/app/core/infrastructure/mappers/product.mapper.ts

import { Product, PriceTier, priceLabel } from '../../domain/models/product.model';

export interface ProductDto {
  id: string;
  slug: string;
  sku: string;
  name: string;
  // ... todos los campos del API
}

export function mapProductDtoToDomain(dto: ProductDto): Product {
  return {
    id: dto.id,
    slug: dto.slug,
    sku: dto.sku,
    name: dto.name,
    brand: dto.brand,
    prices: dto.prices.map(p => ({
      label: p.label as priceLabel,
      price: p.price,
      minQuantity: p.minQuantity
    })),
    // ... mapear todos los campos
  };
}
```

### Paso 3: Crear el Repository

```typescript
// src/app/core/infrastructure/repositories/products.repository.ts

import { inject, Injectable } from '@angular/core';
import { map, Observable } from 'rxjs';
import { ApiHttpService } from '../http/api.service';
import { Product } from '../../domain/models/product.model';
import { ProductDto, mapProductDtoToDomain } from '../mappers/product.mapper';

@Injectable({ providedIn: 'root' })
export class ProductsRepository {
  private api = inject(ApiHttpService);

  getBySlug(slug: string): Observable<Product | null> {
    return this.api.get<ProductDto[]>('products', { slug }).pipe(
      map((list) => {
        const dto = list[0];
        return dto ? mapProductDtoToDomain(dto) : null;
      })
    );
  }
}
```

### Paso 4: Crear el Application Service

```typescript
// src/app/core/application/products/product-details.application-service.ts

import { inject, Injectable, signal, computed } from '@angular/core';
import { toSignal, toObservable } from '@angular/core/rxjs-interop';
import { switchMap, of } from 'rxjs';
import { ProductsRepository } from '../../infrastructure/repositories/products.repository';
import { Product } from '../../domain/models/product.model';

@Injectable({ providedIn: 'root' })
export class ProductDetailsApplicationService {
  private repo = inject(ProductsRepository);
  
  private currentSlug = signal<string>('');

  private productSignal = toSignal(
    toObservable(this.currentSlug).pipe(
      switchMap(slug => slug ? this.repo.getBySlug(slug) : of(null))
    ),
    { initialValue: null }
  );

  private loadingSignal = signal<boolean>(false);
  private errorSignal = signal<string | null>(null);

  readonly product = computed(() => this.productSignal());
  readonly isLoading = computed(() => this.loadingSignal());
  readonly error = computed(() => this.errorSignal());

  setSlug(slug: string) {
    this.loadingSignal.set(true);
    this.errorSignal.set(null);
    this.currentSlug.set(slug);
    
    this.repo.getBySlug(slug).subscribe({
      next: () => {
        this.loadingSignal.set(false);
      },
      error: (err) => {
        this.loadingSignal.set(false);
        this.errorSignal.set(err.message || 'Error al cargar el producto');
      }
    });
  }
}
```

### Paso 5: Crear el Componente

```typescript
// src/app/features/user/product-detail/product-detail.component.ts

import { Component, inject, effect } from '@angular/core';
import { ActivatedRoute } from '@angular/router';
import { ProductDetailsApplicationService } from '@core/application/products/product-details.application-service';
import { CommonModule, CurrencyPipe } from '@angular/common';

@Component({
  selector: 'app-product-detail',
  standalone: true,
  imports: [CommonModule, CurrencyPipe],
  templateUrl: './product-detail.component.html',
  styleUrl: './product-detail.component.css'
})
export class ProductDetailComponent {
  private route = inject(ActivatedRoute);
  productService = inject(ProductDetailsApplicationService);
  
  constructor() {
    effect(() => {
      const slug = this.route.snapshot.paramMap.get('slug');
      if (slug) {
        this.productService.setSlug(slug);
      }
    });
  }
}
```

---

## API de Pruebas con json-server

### Instalación

```bash
# Instalar en el workspace del frontend
cd frontend/stockmaster-client
bun add -D json-server
```

### Configuración

**1. Crear `db.json` en la raíz del frontend:**

```json
{
  "products": [
    {
      "id": "prod-001",
      "slug": "auriculares-bluetooth-pro",
      "sku": "AUD-BT-2024-001",
      "name": "Auriculares Bluetooth Pro",
      "brand": "SoundTech",
      "prices": [
        { "label": "unit", "price": 89.99 }
      ],
      "images": ["https://..."],
      "isActive": true,
      "stockUnits": 120
    }
  ]
}
```

**2. Agregar script en `package.json` del frontend:**

```json
{
  "scripts": {
    "mock": "json-server --watch db.json --port 3001"
  }
}
```

**3. Agregar script en `package.json` raíz:**

```json
{
  "scripts": {
    "dev:mock-api": "bun --cwd frontend/stockmaster-client run mock"
  }
}
```

### Uso

```bash
# Terminal 1 - Iniciar API mock
bun run dev:mock-api

# Terminal 2 - Iniciar frontend
bun run dev:frontend
```

### Endpoints disponibles

- `GET http://localhost:3001/products` - Lista todos
- `GET http://localhost:3001/products?slug=auriculares-bluetooth-pro` - Buscar por slug
- `GET http://localhost:3001/products/prod-001` - Buscar por ID

---

## Ejemplo Completo: Detalle de Producto

### Configurar la Ruta

```typescript
// app.routes.ts
import { Routes } from '@angular/router';
import { ProductDetailComponent } from './features/user/product-detail/product-detail.component';

export const routes: Routes = [
  {
    path: 'products/:slug',
    component: ProductDetailComponent
  }
];
```

### Probar en el navegador

```
http://localhost:4200/products/auriculares-bluetooth-pro
```

---

## Buenas Prácticas

### ✅ DO (Hacer)

1. **Domain Models**: Mantenerlos puros, sin dependencias de Angular
2. **Repositories**: Un repositorio por entidad/agregado
3. **Mappers**: Siempre mapear DTO → Domain en Infrastructure
4. **Application Services**: Manejar estado y orquestar lógica
5. **Componentes**: Solo presentación, delegar lógica a services

### ❌ DON'T (No hacer)

1. **No mezclar capas**: Domain no debe importar de Infrastructure
2. **No lógica en componentes**: Máximo operaciones de presentación
3. **No DTOs en Application**: Solo trabajar con modelos del Domain
4. **No llamadas HTTP directas**: Siempre a través de Repositories
5. **No state management en componentes**: Usar Application Services

### Flujo de Datos

```
Usuario interactúa con UI
         ↓
    Componente llama Application Service
         ↓
    Application Service usa Repository
         ↓
    Repository llama API y mapea DTO → Domain
         ↓
    Domain Model regresa a Application Service
         ↓
    Signals/Observables actualizan UI
```

### Dependencias Permitidas

```
Domain ← Application ← Infrastructure
  ↑                         ↓
  └─────────────────────────┘
           Features (UI)
```

- **Domain**: No depende de nadie (núcleo)
- **Application**: Solo depende de Domain
- **Infrastructure**: Depende de Domain (para mapear)
- **Features/UI**: Puede depender de todas

---

## Migración a Backend Real

Cuando tengas el backend listo:

1. **Cambiar URL en configuración:**
   ```typescript
   // environment.ts
   apiUrl: 'http://localhost:3000'  // En lugar de 3001
   ```

2. **Desinstalar json-server:**
   ```bash
   cd frontend/stockmaster-client
   bun remove json-server
   ```

3. **Eliminar scripts mock:**
   - Borrar script `"mock"` del package.json frontend
   - Borrar script `"dev:mock-api"` del package.json raíz

4. **Eliminar db.json**

5. **¡Listo!** El resto del código no necesita cambios gracias a la arquitectura en capas.

---

## Referencias

- [Clean Architecture - Robert C. Martin](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)
- [Angular Signals](https://angular.dev/guide/signals)
- [json-server Documentation](https://github.com/typicode/json-server)

---

**Última actualización**: Diciembre 2024  
**Versión Angular**: 21.0.0  
**Versión json-server**: 1.0.0-beta.3
