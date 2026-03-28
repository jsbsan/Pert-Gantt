# **Documentación del Proyecto: Calculadora PERT / CPM Web**

**Autor:** Julio Sánchez Berro

**Tipo de Proyecto:** Aplicación Web de una sola página (Single Page Application \- SPA)

**Tecnologías:** HTML5, CSS3 (Tailwind CSS), JavaScript (Vanilla), Vis-Network (Librería de grafos).

## **1\. Descripción General**

La "Calculadora PERT / CPM" es una herramienta web interactiva diseñada para la gestión de proyectos. Permite a los usuarios registrar una serie de tareas con sus estimaciones de tiempo y dependencias, para luego calcular matemáticamente los tiempos de ejecución, los márgenes de retraso (holgura) y visualizar la **Ruta Crítica** del proyecto.

Todo el sistema está contenido en un único archivo index.html, lo que significa que no requiere instalación de software ni servidores (funciona directamente en cualquier navegador web moderno).

## **2\. Funcionalidades Principales**

### **📊 Gestión de Tareas**

* **Entrada de datos:** Formulario para introducir el ID de la tarea, Nombre, Tiempos (Optimista, Probable y Pesimista) y Dependencias (Predecesores).  
* **Edición y Borrado:** Capacidad de editar tareas existentes recuperando sus datos en el formulario, o eliminarlas individualmente/en bloque.  
* **Tabla Reactiva:** Visualización en tiempo real de todas las tareas registradas antes de proceder al cálculo.

### **🧮 Motor de Cálculo (PERT / CPM)**

* **Fórmula PERT:** Calcula el Tiempo Esperado (TE) mediante la distribución Beta: (Optimista \+ 4\*Probable \+ Pesimista) / 6\.  
* **Forward Pass (Hacia adelante):** Calcula el Inicio Temprano (ES) y Fin Temprano (EF).  
* **Backward Pass (Hacia atrás):** Calcula el Inicio Tardío (LS) y Fin Tardío (LF).  
* **Holgura:** Determina el margen de retraso de cada tarea (LS \- ES). Las tareas con holgura cero conforman la **Ruta Crítica**.  
* **Protección de Errores:** Detección de dependencias circulares y referencias a tareas inexistentes.

### **📥 Importación y Exportación**

* **Exportar a CSV:** Genera un archivo .csv compatible con Excel que incluye todos los datos introducidos y los resultados calculados.  
* **Importar desde CSV:** Permite cargar proyectos enteros leyendo archivos .csv con un formato específico, ignorando cabeceras y manejando dependencias múltiples entre comillas.  
* **Plantilla de descarga:** Facilita al usuario un archivo CSV base para rellenar.

### **🎨 Visualización y Gráficos**

* **Diagrama de Red PERT:** Generado dinámicamente con vis-network. Muestra los nodos con toda su información matemática (ES, EF, LS, LF, Holgura) e interconecta las dependencias. Resalta en rojo los nodos y flechas de la ruta crítica.  
* **Diagrama de Gantt:** Gráfico construido de forma nativa superponiendo barras HTML sobre una cuadrícula de tiempo. Incluye flechas direccionales (SVG) que conectan las tareas dependientes y sombreado para visualizar las holguras.  
* **Exportación a SVG:** Posibilidad de guardar tanto el diagrama PERT como el diagrama de Gantt en formato de imagen vectorial (.svg), conservando la máxima calidad para presentaciones.

### **🖥️ Interfaz de Usuario (UI/UX)**

* **Sistema de Pestañas (Tabs):** Organización en tres secciones limpias: "Gestión de Tareas", "Resultados" y "Ayuda".  
* **Controles Visuales (Checkboxes):** Permiten al usuario mostrar/ocultar los IDs, ocultar los textos dentro de las barras del Gantt o esconder las flechas de conexión para limpiar la vista.  
* **Zoom de Paneles:** Botones de lupa (+ y \-) en las cabeceras de cada sección para ampliar o reducir el contenido a gusto del usuario.  
* **Adaptación a Impresión:** Reglas CSS específicas (@media print) para garantizar que al imprimir (Ctrl+P) se mantengan los colores (rojo/azul) y se oculten elementos innecesarios como botones.

## **3\. Arquitectura del Código**

El proyecto está estructurado de la siguiente forma en el archivo index.html:

1. **\<head\>:** Importación de Tailwind (CDN), librería Vis-Network y estilos personalizados (especialmente los de impresión).  
2. **\<body\>:**  
   * **Cabecera (Header):** Títulos, autor, botón general de "Calcular" y checkboxes de visualización global.  
   * **Navegación:** Pestañas para cambiar entre Gestión, Resultados y Ayuda.  
   * **Tab de Gestión:** Contiene el grid responsivo para el formulario, botones de CSV y la tabla cruda.  
   * **Tab de Resultados:** Aparece dinámicamente tras calcular. Muestra la tabla de resultados matemáticos y alberga los contenedores de los gráficos PERT y Gantt.  
   * **Tab de Ayuda:** Texto estático con la guía de conceptos para el usuario.  
3. **\<script\>:** \* Lógica de interfaz (Pestañas, Zoom, Alertas Modales en lugar de alert() del navegador).  
   * Gestión del estado (tareasData y ultimoResultado).  
   * Algoritmo de cálculo (calcularPERT(), clases de objetos).  
   * Parseo y formateo de archivos CSV.  
   * Funciones de renderizado gráfico (dibujarRed(), dibujarGantt(), dibujarFlechasGantt()).  
   * Funciones de exportación SVG (exportarPERTSVG(), exportarGanttSVG()).

## **4\. Historial de Desarrollo (Prompts utilizados)**

El desarrollo del proyecto se llevó a cabo de forma progresiva e iterativa respondiendo a las siguientes peticiones (prompts):

1. *Haz un script en python para calcular el diagrama de pert*  
2. *Añadele un entorno visua*  
3. *hazlo en formato html y javascript* (Conversión a plataforma Web)  
4. *Añade que se pueda ver el diagrama de la red del pert, marcando la ruta crítica.*  
5. *Añade tambien un diagrama gantt*  
6. *Añade un botón para exportar toda la información a un archivos .csv*  
7. *Al imprimir la pagina web, el diagrama de gant no sale coloreado. Revisalo*  
8. *Añade un botón para poder registrar las tareas desde un fichero csv...*  
9. *Verifica que los calculos de los distintos valores del pert son correctos.*  
10. *Quiero que en el diagrama pert, aparezca tambien el nombre de la actividad.*  
11. *Haz que sea opcional (con un checkbox) que se ponga o no el ID en los gráficos del pert y en el diagrama de gantt*  
12. *Que tambien opcional que aparezca el nombre de la actividad en las barras del diagrama de gantt*  
13. *Quiero que haya un botón para guardar tanto el diagrama de red pert como el diagrama de gantt en formato .svg*  
14. *Quiero que, ademas de la acción Borrar, podamos editar una tarea mediante un formulario.*  
15. *Quiero que en el titulo... pongas: "Autor: Julio Sánchez Berro"*  
16. *dame una lista de todos los prompt que he usado en esta conversación.*  
17. *Quiero que al diagrama de gantt, le añada flechas que unan las actividades...*  
18. *revisa el código, parece que te has olvidado de parte.* (Unificación del código)  
19. *Quiero que organices los paneles de la aplilcación como la imagen adjunta.* (Maquetación 2 columnas)  
20. *Añada que se puedan poner 3 columnas, ahora esta fija en 2*  
21. *añade que se pueda cambiar el ancho de las columnas* (Implementación de resizers)  
22. *Añade un boton de zoom mas y zoom menos para reducir el tamaño del contenido de cada panel...*  
23. *Quiero que los botones y check box aparezcan en la barra superior.*  
24. *Vamos a organizar otra vez los paneles: Quiero que haya dos pestañas en la aplicación...* (Estructura final de Tabs)  
25. *Quiero que añadas otra pestaña para la ayuda llamada "Ayuda"...*  
26. *Revisa el código porque la pestaña "Resultado" y "Ayuda" no muestran nada...* (Corrección del HTML cortado)  
27. *dame una lista de todos los prompt que he usado en esta conversación.*  
28. *puedes crear un fichero de texto con todo el contexto de este proyecto para poderlo leer.* (Este documento actual)