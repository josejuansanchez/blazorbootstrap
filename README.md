# blazorbootstrap

Repositorio con ejemplos sobre cómo usar componentes de interfaz con [BlazorBootstrap][1].

## Índice

- [Introducción](#introducción)
- [Requisitos previos](#requisitos-previos)
- **Ejemplos**
  - [Ejemplo 1. Alertas y botones](#ejemplo-1-alertas-y-botones)
  - [Ejemplo 2. Uso de modales](#ejemplo-2-uso-de-modales)
  - [Ejemplo 3. Tarjetas responsivas con información de usuario](#ejemplo-3-tarjetas-responsivas-con-información-de-usuario)
  - [Ejemplo 4. Formulario con validación](#ejemplo-4-formulario-con-validación)
  - [Ejemplo 5. Listado de usuarios con Grid](#ejemplo-5-listado-de-usuarios-con-grid)
- [Ejercicios propuestos](#ejercicios-propuestos)
- [Referencias](#referencias)

---

## Introducción

En esta práctica aprenderemos a utilizar la biblioteca **[BlazorBootstrap][1]**, una librería de componentes creada específicamente para **Blazor Server y Blazor WebAssembly**, que permite aprovechar la potencia de **[Bootstrap 5][2]** sin necesidad de escribir código JavaScript o HTML adicional.

El objetivo de estos ejemplos es familiarizarse con los componentes visuales más comunes: **alertas, botones, modales, tarjetas, formularios y grids**, así como comprender su integración dentro del flujo interactivo de una aplicación Blazor.

---

## Instalación de BlazorBootstrap

Siga los pasos que se detallan en la página oficial de [BlazorBootstrap](https://docs.blazorbootstrap.com/getting-started/blazor-webapp-server-global-net-8) para instalar la librería en su proyecto Blazor.

<!--
```bash
dotnet add package Blazor.Bootstrap
```
-->

---

## Ejemplo 1. Alertas y botones

En este primer ejemplo se muestran las **alertas** (`<Alert>`) y **botones** (`<Button>`) básicos de [BlazorBootstrap][1], junto con la gestión de eventos desde código C#.

**Paso 1. Crear un nuevo componente Razor**

Cree un nuevo archivo Razor llamado `Ejemplo1.razor` en la carpeta `Pages` de su proyecto Blazor.

**Paso 2. Añadir el código del ejemplo**

Añada el siguiente código al archivo `Ejemplo1.razor`:

```razor
@page "/ejemplo1"
@rendermode InteractiveServer

<h3>Ejemplo 1. Alertas y Botones</h3>

<!-- Alerta fija con botón de cerrar -->
<Alert Color="AlertColor.Success" Dismissable="true">
    Alerta sin icono que se puede cerrar
</Alert>

<Alert Color="AlertColor.Danger" Dismissable="true">
    <Icon Name="IconName.ExclamationTriangleFill" class="me-2"></Icon>
    Alerta con icono que se puede cerrar
</Alert>

<!-- Botón que muestra/oculta la alerta informativa -->
<Button Color="ButtonColor.Primary" Type="ButtonType.Button" class="me-2" @onclick="ToggleAlerta">
    Mostrar / Ocultar alerta informativa
</Button>

@if (mostrarInfo)
{
    <Alert Color="AlertColor.Info" Dismissable="false" class="mt-3">
        <Icon Name="IconName.InfoCircleFill" class="me-2"></Icon>
        Esta alerta se muestra dinámicamente desde C#
    </Alert>
}

<p class="mt-2"><strong>mostrarInfo:</strong> @mostrarInfo</p>

@code {
    private bool mostrarInfo = true;

    private void ToggleAlerta()
    {
        mostrarInfo = !mostrarInfo;
        Console.WriteLine("Botón ToggleAlerta pulsado");
    }
}
```

### Comentarios

* `<Alert>`: Componente que muestra mensajes visuales de diferentes colores (`Success`, `Danger`, `Info`, etc.).
* `Dismissable="true"`: Es un atributo del componente `Alert` que añade un botón en la esquina superior derecha para cerrar la alerta.
* `<Icon>`: Inserta un icono Bootstrap. Existen muchos tipos, por ejemplo: `ExclamationTriangleFill`, `InfoCircleFill`, etc.
* `<Button>`: Componente que crea un botón configurable por color y tipo.

**Paso 3. Modificar el archivo de navegación**

Añade un enlace al nuevo componente en Components/Layout/NavMenu.razor:

```razor
<div class="nav-item px-3">
    <NavLink class="nav-link" href="ejemplo1">
        <span class="bi bi-list-nested-nav-menu" aria-hidden="true"></span> Ejemplo 1
    </NavLink>
</div>          
```

---

## Ejemplo 2. Uso de modales

Este ejemplo muestra cómo utilizar el componente `<Modal>` de [BlazorBootstrap][1], que permite abrir ventanas emergentes con contenido y acciones personalizadas.

**Paso 1. Crear un nuevo componente Razor**

Cree un nuevo archivo Razor llamado `Ejemplo2.razor` en la carpeta `Pages` de su proyecto Blazor.

**Paso 2. Añadir el código del ejemplo**

```razor
@page "/ejemplo2"
@rendermode InteractiveServer

<h3>Ejemplo 2. Modal con Blazor Bootstrap</h3>

<Modal @ref="modal" Title="Título del Modal">
    <BodyTemplate>
        El contenido del modal va aquí.
    </BodyTemplate>
    <FooterTemplate>
        <Button Color="ButtonColor.Secondary" @onclick="OnHideModalClick">Cerrar</Button>
        <Button Color="ButtonColor.Primary">Guardar cambios</Button>
    </FooterTemplate>
</Modal>

<Button Color="ButtonColor.Primary" @onclick="OnShowModalClick">Mostrar Modal</Button>

@code {
    private Modal modal = default!;

    private async Task OnShowModalClick() => await modal.ShowAsync();
    private async Task OnHideModalClick() => await modal.HideAsync();
}
```

### Comentarios

* `<Modal>`: Componente que define la ventana modal.

  * `@ref="modal"`: Referencia al componente para manipularlo desde C#.
* `<BodyTemplate>`: Contenido principal de la ventana modal.
* `<FooterTemplate>`: Parte inferior del modal, normalmente con botones de acción.
* `modal.ShowAsync()` / `modal.HideAsync()`: Métodos que muestran u ocultan la ventana modal.

**Paso 3. Modificar el archivo de navegación**

Añade un enlace al nuevo componente en `Components/Layout/NavMenu.razor`:

```razor
<div class="nav-item px-3">
    <NavLink class="nav-link" href="ejemplo2">
        <span class="bi bi-list-nested-nav-menu" aria-hidden="true"></span> Ejemplo 2
    </NavLink>
</div>
```

---

## Ejemplo 3. Tarjetas responsivas con información de usuario

Este ejemplo utiliza el componente `<Card>` para mostrar información de varios usuarios en un diseño responsive.

**Paso 1. Crear un nuevo componente Razor**

Cree un nuevo archivo Razor llamado `Ejemplo3.razor` en la carpeta `Pages` de su proyecto Blazor.

**Paso 2. Añadir el código del ejemplo**

```razor
@page "/ejemplo3"
@rendermode InteractiveServer

<h3>Ejemplo 3. Tarjetas responsivas con información de usuario</h3>

<div class="row g-3">
    @foreach (var usuario in usuarios)
    {
        <div class="col-12 col-md-6 col-lg-4">
            <Card Class="h-100 shadow-sm">
                <CardHeader>
                    <strong>@usuario.Nombre</strong>
                </CardHeader>

                <CardBody>
                    <img src="@usuario.Imagen"
                         class="img-fluid rounded-circle mb-3"
                         alt="@usuario.Nombre" width="120" />
                    <p class="fw-semibold mb-1">@usuario.Rol</p>

                    <div class="border-top pt-2 mt-2 small">
                        <p class="mb-1"><strong>Correo:</strong> @usuario.Email</p>
                        <p class="mb-1"><strong>Teléfono:</strong> @usuario.Telefono</p>
                        <p class="mb-0"><strong>Descripción:</strong> @usuario.Descripcion</p>
                    </div>
                </CardBody>
            </Card>
        </div>
    }
</div>

@code {
    private readonly List<Usuario> usuarios = new()
    {
        new Usuario
        {
            Nombre = "Usuario 1",
            Rol = "Administrador del sistema",
            Email = "admin@empresa.com",
            Telefono = "950 252525",
            Imagen = "https://randomuser.me/api/portraits/men/40.jpg",
            Descripcion = "Responsable de la configuración general del sistema y seguridad."
        },
        new Usuario
        {
            Nombre = "Usuario 2",
            Rol = "Gestor de contenido",
            Email = "content@empresa.com",
            Telefono = "950 242424",
            Imagen = "https://randomuser.me/api/portraits/men/41.jpg",
            Descripcion = "Encargado de la creación y actualización de contenidos en el portal."
        },
        new Usuario
        {
            Nombre = "Usuario 3",
            Rol = "Soporte técnico",
            Email = "soporte@empresa.com",
            Telefono = "950 232323",
            Imagen = "https://randomuser.me/api/portraits/men/42.jpg",
            Descripcion = "Atiende incidencias y da soporte a usuarios internos y externos."
        }
    };

    private class Usuario
    {
        public string Nombre { get; set; } = string.Empty;
        public string Rol { get; set; } = string.Empty;
        public string Email { get; set; } = string.Empty;
        public string Telefono { get; set; } = string.Empty;
        public string Imagen { get; set; } = string.Empty;
        public string Descripcion { get; set; } = string.Empty;
    }
}
```

### Comentarios

* `<Card>`: Contenedor visual con borde y sombra (`shadow-sm`).
* `<CardHeader>`: Parte superior de la tarjeta, usada para el nombre del usuario.
* `<CardBody>`: Contenido principal de la tarjeta con imagen, texto y datos secundarios.
* Las clases Bootstrap (`col-12`, `col-md-6`, `col-lg-4`) hacen que la cuadrícula sea **responsive**.

| Clase | Breakpoint | Significado | Resultado visual |
| :--- | :--- |:--- | :--- |
| `col-12`   | **Extra pequeño (XS)**: pantallas menores de 576 px (móviles) | La columna ocupa **todo el ancho disponible (12 columnas)**      | Una sola tarjeta por fila |
| `col-md-6` | **Mediano (MD)**: >= 768 px (tablets)                          | La columna ocupa **6 de 12 columnas** (la mitad del contenedor)  | Dos tarjetas por fila     |
| `col-lg-4` | **Grande (LG)**: >= 992 px (ordenadores de escritorio)         | La columna ocupa **4 de 12 columnas** (un tercio del contenedor) | Tres tarjetas por fila    |


**Paso 3. Modificar el archivo de navegación**

Añade un enlace al nuevo componente en `Components/Layout/NavMenu.razor`:

```razor
<div class="nav-item px-3">
    <NavLink class="nav-link" href="ejemplo3">
        <span class="bi bi-list-nested-nav-menu" aria-hidden="true"></span> Ejemplo 3
    </NavLink>
</div>     
``` 

---

## Ejemplo 4. Formulario con validación

En este ejemplo se implementa un formulario controlado con **validaciones automáticas** mediante `DataAnnotations` y componentes para la entrada de datos de BlazorBootstrap.

**Paso 1. Crear un nuevo componente Razor**

Cree un nuevo archivo Razor llamado `Ejemplo4.razor` en la carpeta `Pages` de su proyecto Blazor.

**Paso 2. Añadir el código del ejemplo**

```razor
@page "/ejemplo4"
@using System.ComponentModel.DataAnnotations
@rendermode InteractiveServer

<h3>Ejemplo 4. Formulario con validación</h3>

<EditForm Model="@usuario" OnValidSubmit="Guardar">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <div class="mb-3">
        <label for="nombre" class="form-label">Nombre</label>
        <InputText id="nombre" class="form-control" @bind-Value="usuario.Nombre" />
        <ValidationMessage For="@(() => usuario.Nombre)" />
    </div>

    <div class="mb-3">
        <label for="email" class="form-label">Correo electrónico</label>
        <InputText id="email" class="form-control" @bind-Value="usuario.Email" />
        <ValidationMessage For="@(() => usuario.Email)" />
    </div>

    <div class="mb-3">
        <label for="fechaNacimiento" class="form-label">Fecha de nacimiento:</label>
        <DateInput TValue="DateOnly?" id="fechaNacimiento" class="form-control" @bind-Value="@usuario.FechaNacimiento" Placeholder="Introduce una fecha" />
        <ValidationMessage For="@(() => usuario.FechaNacimiento)" />
    </div>

    <Button Color="ButtonColor.Success" Type="ButtonType.Submit">Guardar</Button>
</EditForm>

@if (mostrarAlerta)
{
    <Alert Color="AlertColor.Success" Dismissable="true" class="mt-3">
        <Icon Name="IconName.CheckCircleFill" class="me-2"></Icon>
        Usuario guardado correctamente
    </Alert>
}

@code {
    private Usuario usuario = new();
    private bool mostrarAlerta = false;

    public class Usuario
    {
        [Required(ErrorMessage = "El nombre es obligatorio")]
        public string Nombre { get; set; } = string.Empty;

        [Required(ErrorMessage = "Formato de correo no válido")]
        public string Email { get; set; } = string.Empty;

        [Required(ErrorMessage = "La fecha de nacimiento es obligatoria")]
        public DateOnly? FechaNacimiento { get; set; } = DateOnly.FromDateTime(DateTime.Now);
    }

    private async Task Guardar()
    {
        mostrarAlerta = true;
    }
}
```

### Comentarios

* `<EditForm>`: Controla el formulario y valida el modelo.
* `<DataAnnotationsValidator>`: Activa la validación basada en atributos.
* `<ValidationMessage>`: Muestra los mensajes de error específicos.
* `<DateInput>`: Componente de BlazorBootstrap para fechas (`DateOnly`).
* `<Alert>`: Muestra un mensaje de éxito tras guardar.


**Paso 3. Modificar el archivo de navegación**

Añade un enlace al nuevo componente en `Components/Layout/NavMenu.razor`:

```razor
<div class="nav-item px-3">
    <NavLink class="nav-link" href="ejemplo4">
        <span class="bi bi-list-nested-nav-menu" aria-hidden="true"></span> Ejemplo 4
    </NavLink>
</div>
```

---

## Ejemplo 5. Listado de usuarios con Grid

Este último ejemplo utiliza el componente `<Grid>` para mostrar, filtrar, ordenar y paginar registros dinámicamente.

**Paso 1. Crear un nuevo componente Razor**

Cree un nuevo archivo Razor llamado `Ejemplo5.razor` en la carpeta `Pages` de su proyecto Blazor.

**Paso 2. Añadir el código del ejemplo**

```razor
@page "/ejemplo5"
@using System.Globalization

<h3>Ejemplo 5. Listado de usuarios</h3>

<Grid TItem="Usuario"
      AllowFiltering="true"
      AllowPaging="true"
      AllowSorting="true"
      AllowSelection="true"
      Class="table table-hover table-bordered table-striped"
      DataProvider="UsuariosDataProvider"
      PageSize="5"
      Responsive="true"
      RowKeySelector="u => u.Id"
      SelectedItemsChanged="OnSelectedItemsChanged"
      SelectionMode="GridSelectionMode.Multiple">

    <GridColumns>
        <GridColumn TItem="Usuario" HeaderText="ID">@context.Id</GridColumn>
        <GridColumn TItem="Usuario" HeaderText="Nombre">@context.Nombre</GridColumn>
        <GridColumn TItem="Usuario" HeaderText="Correo electrónico">@context.Email</GridColumn>
        <GridColumn TItem="Usuario" HeaderText="Fecha de nacimiento">
            @context.FechaNacimiento.ToString("dd/MM/yyyy", CultureInfo.InvariantCulture)
        </GridColumn>
    </GridColumns>
</Grid>

<div class="mt-3">
    <strong>Usuarios seleccionados:</strong> @selectedUsuarios.Count
</div>

@if (selectedUsuarios.Any())
{
    <ul>
        @foreach (var user in selectedUsuarios)
        {
            <li>@user.Nombre (@user.Email)</li>
        }
    </ul>
}

@code {
    private IEnumerable<Usuario> usuarios = default!;
    private HashSet<Usuario> selectedUsuarios = new();

    private async Task<GridDataProviderResult<Usuario>> UsuariosDataProvider(GridDataProviderRequest<Usuario> request)
    {
        usuarios ??= GetUsuarios();
        return await Task.FromResult(request.ApplyTo(usuarios));
    }

    private IEnumerable<Usuario> GetUsuarios() => new List<Usuario>
    {
        new Usuario { Id = 1, Nombre = "Ana Torres", Email = "ana.torres@example.com", FechaNacimiento = new DateOnly(1990, 3, 14) },
        new Usuario { Id = 2, Nombre = "Luis García", Email = "luis.garcia@example.com", FechaNacimiento = new DateOnly(1988, 7, 9) },
        new Usuario { Id = 3, Nombre = "Marta López", Email = "marta.lopez@example.com", FechaNacimiento = new DateOnly(1995, 12, 25) },
        new Usuario { Id = 4, Nombre = "Carlos Pérez", Email = "carlos.perez@example.com", FechaNacimiento = new DateOnly(1982, 5, 2) },
        new Usuario { Id = 5, Nombre = "Laura Sánchez", Email = "laura.sanchez@example.com", FechaNacimiento = new DateOnly(1999, 10, 21) },
        new Usuario { Id = 6, Nombre = "Javier Ruiz", Email = "javier.ruiz@example.com", FechaNacimiento = new DateOnly(1985, 1, 18) },
        new Usuario { Id = 7, Nombre = "Patricia Gómez", Email = "patricia.gomez@example.com", FechaNacimiento = new DateOnly(1992, 11, 30) },
    };

    private Task OnSelectedItemsChanged(HashSet<Usuario> items)
    {
        selectedUsuarios = items ?? new();
        return Task.CompletedTask;
    }

    public class Usuario
    {
        public int Id { get; set; }
        public string Nombre { get; set; } = string.Empty;
        public string Email { get; set; } = string.Empty;
        public DateOnly FechaNacimiento { get; set; }
    }
}
```

### Comentarios

* `<Grid>`: Tabla avanzada con filtrado, paginación y ordenación integradas.
* `DataProvider`: Función que proporciona los datos al grid.
* `RowKeySelector`: Identifica cada fila por su clave primaria.
* `SelectionMode`: Permite selección múltiple o simple.
* `SelectedItemsChanged`: Evento que se dispara al seleccionar filas.

**Paso 3. Modificar el archivo de navegación**

Añade un enlace al nuevo componente en `Components/Layout/NavMenu.razor`:

```razor
<div class="nav-item px-3">
    <NavLink class="nav-link" href="ejemplo5">
        <span class="bi bi-list-nested-nav-menu" aria-hidden="true"></span> Ejemplo 5
    </NavLink>
</div>
```

---

## Ejercicios propuestos

### Ejercicio 1. Añadir iconos a las tarjetas de usuario

Modifica el componente del **Ejemplo 3** para añadir un icono representativo según el rol del usuario. También puedes cambiar el color del borde de la tarjeta según el rol, por ejemplo, azul para administradores, verde para usuarios normales, etc.

### Ejercicio 2. Validación adicional en el formulario

Amplía el **Ejemplo 4** para incluir nuevos campos como teléfono, dirección, rol, foto, etc. Debe validar estos campos utilizando atributos de validación adicionales.

### Ejercicio 3. Añadir eliminación en el Grid

Modifica el **Ejemplo 5** para incluir una columna con un botón que permita eliminar usuarios del listado.

### Ejercicio 4. Integración de modal de confirmación

Combina el **Ejemplo 2** (Modal) con el **Ejemplo 5** (Grid) para confirmar la eliminación de un usuario seleccionado.

---

## Referencias

* [Documentación oficial de BlazorBootstrap](https://docs.blazorbootstrap.com/)
* [Microsoft Docs: Componentes de Blazor Server](https://learn.microsoft.com/es-es/aspnet/core/blazor/components/?view=aspnetcore-9.0)
* [Bootstrap 5 Documentation](https://getbootstrap.com/docs/5.3/getting-started/introduction/)
* [Repositorio GitHub de BlazorBootstrap](https://github.com/vikramlearning/blazorbootstrap)

[1]: https://demos.blazorbootstrap.com/
[2]: https://getbootstrap.com